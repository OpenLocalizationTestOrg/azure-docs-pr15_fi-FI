<properties
   pageTitle="Luo PowerShellin perinteinen käyttöönoton mallin sisäinen kuormituksen | Microsoft Azure"
   description="Opettele luomaan PowerShellin perinteinen käyttöönoton mallin sisäinen kuormituksen"
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
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Luomisesta sisäinen kuormituksen (perinteinen) PowerShellin avulla

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Luo sisäinen kuormituksen, näennäiskoneiden määrittäminen

Luo sisäinen kuormituksen tasauspalvelun määrittää siihen ja niiden liikenne lähettää sen palvelimet, edellyttää seuraavasti:

1. Luo sisäinen ladata tasaamisen, jotka ovat saapuvan liikenteen on kuormitus tasataan palvelinten kuormituksen joukon päätepisteen esiintymä.

1. Lisää vastaavat näennäiskoneiden, jotka vastaanottaa saapuvan liikenteen päätepisteet.

1. Määritä palvelimet, joilla lähettäminen on kuormitus niiden liikenne lähettää virtual IP (VIP) osoitteeseen sisäinen ladata tasaamisen esiintymän tasataan liikenne.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Vaihe 1: Luo sisäinen kuormituksen tasaamisen esiintymä

Aiemmin pilvipalvelussa tai otettu käyttöön, valitse aluekohtaiset virtual verkosto pilvipalveluun voit luoda sisäinen kuormituksen tasaamisen erillisen Windows PowerShell-komennoilla:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Huomaa, että tämä [Lisää AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShellin cmdlet-komennon käyttäminen DefaultProbe parametrin määrittäminen. Lisätietoja muita parametrin joukot-kohdassa [Lisää AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Vaihe 2: Lisää päätepisteet sisäinen kuormituksen tasaamisen-esiintymä

Tässä on esimerkki:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Vaihe 3: Määritä niiden liikenne lähettää uusi sisäinen kuormituksen tasaamisen päätepiste palvelinten

Sinun on määritettävä palvelimissa, joiden liikenne käyttäjästä tulee kuormitus käyttämään uusi IP-osoite (VIP) sisäinen ladata tasaamisen esiintymän tasataan. Tämä on osoite, jossa sisäinen kuormituksen tasaamisen esiintymän seuraa. Useimmissa tapauksissa haluat vain lisätä tai muokata DNS-tietueen sisäinen kuormituksen tasaamisen esiintymän VIP.

Jos olet määrittänyt IP-osoite sisäinen kuormituksen tasaamisen esiintymän luonnin aikana, sinulla on jo VIP. Muussa tapauksessa näet VIP seuraavat komennot:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Jos haluat käyttää näitä komentoja, täytä arvot ja poista < ja >. Tässä on esimerkki:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Get-AzureInternalLoadBalancer-komennon näytöstä Huomaa IP-osoite ja tee tarvittavat muutokset palvelimia tai varmistaa, että liikenne mene VIP DNS-tietueet.

>[AZURE.NOTE]Microsoft Azure-ympäristön käyttää järjestelmänvalvojan skenaarioita staattinen julkisesti reititettävä IPv4-osoite. IP-osoite on 168.63.129.16. IP-osoitteen estänyt minkä tahansa palomuurit ei olisi, koska se voi aiheuttaa odottamatonta toimintaa.
>Jotka koskevat Azure sisäinen ladata tasaamisen tämä IP-osoite on määrittämiseen seuraamalla keräysputkien-kuormituksen kuntotietojen näennäiskoneiden kuormitus tasataan joukon. Jos verkon käyttöoikeusryhmän rajoitetaan liikenne Azuren näennäiskoneiden sisäisesti kuormituksen saapuva määrittäminen tai otetaan käyttöön Virtual verkko-osoitteiden, varmistaa, että verkon suojaussäännön lisätään sallimaan tietoliikenteen 168.63.129.16.


## <a name="example-of-internal-load-balancing"></a>Esimerkki sisäinen kuormituksen tasaamisen

Vaihe pää-ja end vaiheet luominen kaksi Esimerkki käyttömahdollisuudet kuormituksen Määritä-kohdassa seuraavissa osissa.

### <a name="an-internet-facing-multi-tier-application"></a>Vastakkaisten monitasoisten sovelluksen Internet

Haluat tarjota kuormitus tasataan tietokannan joukon Internetiin yhteydessä oleva verkko-palvelimiin. Molempien palvelinten isännöidään Azure pilvipalvelussa. Web-palvelimen tietoliikenteen porttinumeroa 1433 on voi jakaa tietokannan tason kaksi näennäiskoneiden. Kuvassa 1 on määritykset.

![Sisäinen kuormituksen määrittäminen tietokannan tason](./media/load-balancer-internal-getstarted/IC736321.png)


Kokoonpanon koostuu seuraavasti:

- Aiemmin pilvipalvelussa isännöivän näennäiskoneiden on nimeltään mytestcloud.

- Kahden aiemmin luotu tietokanta palvelimen nimi on MJP1 DB2.

- Web-taso-WWW-palvelimien yhdistäminen tietokannan tason tietokannan palvelimissa yksityinen IP-osoitetta. Toinen vaihtoehto on omia DNS virtual verkon ja rekisteröi manuaalisesti sisäinen kuormituksen tasauspalvelun määrittäminen A-tietue.

Seuraavat komennot Määritä uusi sisäinen kuormituksen tasaamisen esiintymä nimeltä **ILBset** ja lisätä päätepisteet vastaavat kaksi tietokannan palvelimet näennäiskoneiden seuraavasti:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Poista sisäinen kuormituksen tasaamisen määritys

Jos haluat poistaa virtual machine päätepisteen sisäinen kuormituksen tasauspalvelun esiintymästä, käytä seuraavia komentoja:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Jos haluat käyttää näitä komentoja, täytä arvot poistaminen < ja >.

Tässä on esimerkki:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Jos haluat poistaa sisäinen kuormituksen tasauspalvelun esiintymä pilvipalveluun, käytä seuraavia komentoja:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Jos haluat käyttää näitä komentoja, täytä arvo ja poista < ja >.

Tässä on esimerkki:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Lisätietoja sisäinen kuormituksen tasauspalvelun cmdlet-komennot


Hanki lisätietoja sisäinen kuormituksen tasaamisen cmdlet-komennot, suorita Windows PowerShell-kehotteessa seuraavista komennoista:

- Ohjeita uuden AzureInternalLoadBalancerConfig-koko

- Hanki ohjeita Lisää AzureInternalLoadBalancer-koko

- Hanki ohjeita Get-AzureInternalLoadbalancer-koko

- Hanki ohjeita Poista AzureInternalLoadBalancer-koko

## <a name="next-steps"></a>Seuraavat vaiheet

[Lähde-IP-affiniteetti käyttämällä kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)