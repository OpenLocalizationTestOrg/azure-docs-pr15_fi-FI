<properties
    pageTitle="Yleisiä verkon PowerShell komentoja, joiden VMs | Microsoft Azure"
    description="Yleiset PowerShell-komennot, joiden avulla pääset alkuun luominen VMs virtual verkko- ja sen niihin liittyvät resurssit."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>VMs Yleiset verkon PowerShellin Azure komennot

Jos haluat luoda virtual machine, joudut [VPN](../virtual-network/virtual-networks-overview.md) luominen tai aiemmin virtual verkon jossa AM voidaan lisätä seikkoja. Yleensä luodessasi AM myös joudut luominen on kuvattu tämän artikkelin resursseista.

Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja PowerShellin Azure uusimman version asentamisesta, valitsemalla tilauksen ja tiliisi kirjautuminen.

## <a name="create-network-resources"></a>Luo verkkoresursseja

Tehtävä | Komento 
-------------- | -------------------------
Luo aliverkon käyttömahdollisuudet | $subnet1 = [Uusi AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -nimen "subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$subnet2 = uusi AzureRmVirtualNetworkSubnetConfig-nimen "subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Tyypillinen verkon voi olla aliverkon [internet vastakkaisten kuormituksen](../load-balancer/load-balancer-internet-overview.md) ja erillinen aliverkon [sisäinen kuormituksen](../load-balancer/load-balancer-internal-overview.md). |
Luo virtuaalisia verkko | $vnet = [Uusi AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -resource_group_name"nimi"virtual_network_name"- ResourceGroupName"-"location_name" sijainti - AddressPrefix XX. X.X.X/XX-aliverkon $subnet1, $subnet2
Testaa yksilöllinen toimialuenimi | [Testaa AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "toimialueen_nimi"-"location_name" sijainti<BR><BR>Voit määrittää DNS-toimialuenimi [julkiseen IP-resurssi](../virtual-network/virtual-network-ip-addresses-overview-arm.md), joka luo domainname.location.cloudapp.azure.com julkiseen IP-osoitteeseen yhdistämismääritys Azure hallita DNS-palvelimet. Nimessä voi olla vain kirjaimia, numeroita ja väliviivoja. Ensimmäisen ja viimeisen merkin on oltava kirjain tai numero ja toimialuenimen on oltava yksilöllinen Azure paikkaan. Jos **Tosi** , ehdotettu nimi on yksilöivä.
Luo julkinen IP-osoite | $pip = [Uusi AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -nimen "ip_address_name" - ResourceGroupName "resource_group_name" - DomainNameLabel "toimialueen_nimi"-"location_name" sijainti - AllocationMethod dynaaminen<BR><BR>Julkinen IP-osoite käyttää aiemmin testattu ja käytetään kuormituksen edusta-määrityksestä toimialuenimeä.
Luo edusta-IP-määritys | $frontendIP = [Uusi AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -nimen "frontend_ip_name" - PublicIpAddress $pip<BR><BR>Edusta-määritys on julkinen IP-osoite, jonka loit aiemmin saapuvaa verkkoliikennettä.
Luo Taustajärjestelmä osoite resurssivarantoon | $beAddressPool = [Uusi AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -nimen "backend_pool_name"<BR><BR>Sisältää sisäisiä osoitteita, joita voi käyttää verkkoliittymän kautta kuormituksen Taustajärjestelmä.
Luo näytteenottimen | $healthProbe = [Uusi AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -nimen "probe_name" - RequestPath 'HealthProbe.aspx'-protokollan http-portin 80 – IntervalInSeconds 15 - ProbeCount 2<BR><BR>Sisältää kunto keräysputkien käytettävä näennäiskoneiden esiintymien Taustajärjestelmä osoite sarjassa käytettävyyden tarkistaminen.
Kuormituksen säännön luominen | $lbRule = [Uusi AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -nimen HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-tutkia $healthProbe-protokollan Tcp - FrontendPort 80 – BackendPort 80<BR><BR>Sisältää sääntöjä, joka määrittää kuormituksen julkisen porttia Taustajärjestelmä osoite resurssivarantoon porttiin.
Saapuvan NAT-säännön luominen | $inboundNATRule = [Uusi AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -nimen "rule_name" - FrontendIpConfiguration $frontendIP-protokollan TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Sisältää yhdistäminen julkisen porttiin kuormituksen tiettyä virtual konetta Taustajärjestelmä osoite sarjassa portin sääntöjä.
Luo kuormituksen tasaus | $loadBalancer = [Uusi AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName "resource_group_name"-nimen "load_balancer_name"-"location_name" sijainti - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-tutkia $healthProbe
Luo verkko-käyttöliittymä | $nic1 = [Uusi AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName "resource_group_name"-nimen "network_interface_name"-"location_name" sijainti - PrivateIpAddress XX. X.X.X-aliverkon subnet2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Luo julkinen IP-osoite ja VPN-aliverkon aiemmin luomasi verkoston käyttöliittymä.
    
## <a name="get-information-about-network-resources"></a>Tietoja verkkoresursseja

Tehtävä | Komento 
-------------- | -------------------------
Luettelon virtual verkot | [Hae AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Näyttää resurssiryhmän virtual verkot.
Virtuaalinen verkon koskevien tietojen hakeminen | Hae AzureRmVirtualNetwork-resource_group_name"nimi"virtual_network_name"- ResourceGroupName"
Luettelon aliverkosta virtual verkossa | Hae AzureRmVirtualNetwork-nimi "virtual_network_name" - ResourceGroupName "resource_group_name" & #124; Valitse aliverkosta
Aliverkon koskevien tietojen hakeminen | [Hae AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -nimen "subnet_name" - VirtualNetwork $vnet<BR><BR>Tietoja aliverkon saa määritettyä virtual verkossa. $Vnet arvo vastaa Get-AzureRmVirtualNetwork palauttama objekti.
Luettelon IP-osoitteet | [Hae AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Näyttää resurssiryhmän julkiseen IP-osoitteet.
Luettelon kuormituksen tasoitusmääritykset | [Hae AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Näyttää kaikki resurssiryhmän kuormituksen tasoitusmääritykset.
Luettelon verkon liityntäkohdat | [Hae AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Näyttää kaikki resurssiryhmän Verkkoliittymät.
Tietoja verkko-käyttöliittymä | Hae AzureRmNetworkInterface-resource_group_name"nimi"network_interface_name"- ResourceGroupName"<BR><BR>Hakee tietoja tietyn Verkkokäyttöliittymän.
Hae verkon liittymän IP-määritys | [Hae AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -nimen "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Hakee tietoja verkon liittymän IP-määritys. $Nic arvo vastaa Get-AzureRmNetworkInterface palauttama objekti.

## <a name="manage-network-resources"></a>Verkkoresurssien hallinta

Tehtävä | Komento 
-------------- | -------------------------
Lisää aliverkon virtual verkkoon | [Lisää AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX-nimen "subnet_name" - VirtualNetwork $vnet<BR><BR>Lisää aliverkon virtual verkkoon. $Vnet arvo vastaa Get-AzureRmVirtualNetwork palauttama objekti.
Poista virtuaalikoneen verkko | [Poista AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -resource_group_name"nimi"virtual_network_name"- ResourceGroupName"<BR><BR>Poistaa tietyn virtual verkon resurssiryhmän.
Poista verkko-käyttöliittymä | [Poista AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -resource_group_name"nimi"network_interface_name"- ResourceGroupName"<BR><BR>Poistaa määritetyn verkkoliittymän resurssiryhmän.
Poista kuormituksen tasaus | [Poista AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -resource_group_name"nimi"load_balancer_name"- ResourceGroupName"<BR><BR>Poistaa määritetyn kuormituksen resurssiryhmän.
Poista julkisen IP-osoite | [Poista AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-resource_group_name"nimi"ip_address_name"- ResourceGroupName"<BR><BR>Poistaa resurssiryhmän määritetty julkiseen IP-osoite.

## <a name="next-steps"></a>Seuraavat vaiheet

- Käytä juuri luomasi kun verkko [AM luominen](virtual-machines-windows-ps-create.md).
- Lue lisätietoja siitä, miten voit [luoda useita verkko-liittymät AM](../virtual-network/virtual-networks-multiple-nics.md).
