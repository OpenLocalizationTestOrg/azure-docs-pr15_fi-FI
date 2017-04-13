<properties
    pageTitle="Luo Internet vastakkaisten kuormituksen IPv6: N kanssa, resurssien hallinta PowerShellin avulla | Microsoft Azure"
    description="Opettele luomaan selainpohjainen vastakkaisten kuormituksen IPv6: N kanssa, resurssien hallinta PowerShellin avulla"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6: ta, azure kuormituksen, kahden pinon, julkiseen ip, alkuperäisen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Luomisesta selainpohjainen vastakkaisten kuormituksen IPv6: N kanssa, resurssien hallinta PowerShellin avulla

> [AZURE.SELECTOR]
- [PowerShellin](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Malli](./load-balancer-ipv6-internet-template.md)

Azure kuormituksen on kerros-4 (TCP- ja UDP) kuormituksen. Kuormituksen on suuri käytettävyys jakamalla saapuvan liikenteen kunnossa palveluesiintymiä cloud Services-palveluissa tai näennäiskoneiden kuormituksen tasauspalvelun joukon. Azure kuormituksen myös näyttää useita portit, useita IP-osoitteita tai molempia näistä palveluista.

## <a name="example-deployment-scenario"></a>Käyttöönotto-Esimerkkitapaus

Seuraavassa kaaviossa on kuvattu tämän artikkelin käyttöön otettavan ratkaisu kuormituksen.

![Kuormituksen tasauspalvelun-skenaario](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

Tässä skenaariossa luot Azure seuraavissa resursseissa:

- Internetiin yhteydessä oleva-kuormituksen IPv4 ja IPv6 julkiseen IP-osoite
- kaksi ladata julkisen VIPs yhdistäminen yksityinen päätepisteet Vastatilin säännöt
- Käytettävyyden asettaminen, joka sisältää kaksi VMs
- kaksi näennäiskoneiden (VMs)
- VPN-liittymän, kunkin AM määritetty IPv4- ja IPv6-osoitteet

## <a name="deploying-the-solution-using-the-azure-powershell"></a>PowerShellin Azure-ratkaisun käyttöönotto

Seuraavissa vaiheissa kuvataan luomisesta selainpohjainen vastakkaisten kuormituksen PowerShellin Azure resurssien hallinnan avulla. Azure resurssien hallinnan avulla kullekin resurssille on luotu ja määritetty yksitellen, valitse hyllytetty yhteen, kun haluat luoda resurssin.

Ottaa käyttöön kuormituksen luominen ja määrittäminen seuraaville objekteille:

- IP-määritys edusta - sisältää julkisten IP-osoitteiden saapuvaa verkkoliikennettä.
- Taustatietokannan osoiteryhmä - sisältää verkkoliittymät (NIC) vastaanottaa verkkoliikennettä kuormituksen näennäiskoneiden.
- Kuormituksen tasaamisen säännöt - sisältää sääntöjä yhdistäminen julkisen porttiin kuormituksen taustatietokantaan osoite resurssivarantoon-porttiin.
- Saapuvan liikenteen säännöt NAT - sisältää yhdistäminen julkisen porttiin kuormituksen tiettyä virtual konetta taustatietokantaan osoite varannon portin säännöt.
- Probes - sisältää kunto keräysputkien käytettävä näennäiskoneiden esiintymien taustatietokantaan osoite varannon käytettävyyden tarkistaminen.

Lisätietoja on artikkelissa [Azure Resurssienhallinta kuormituksen tuki](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Määritä PowerShell käyttämään resurssien hallinta

Varmista, että sinulla on Azure Resurssienhallinta-moduulin uusimman tuotannon PowerShell.

1. Kirjaudu sisään Azure

        Login-AzureRmAccount

    Anna tunnistetiedot pyydettäessä.

2. Tarkista tilin tilaukset

        Get-AzureRmSubscription

3. Valitse, mitä Azure tilauksistasi käyttämään.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Resurssiryhmä (ohita tämä vaihe, jos aiemmin resurssiryhmä) luominen

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Luo virtuaalisia verkko- ja edusta IP-ryhmän julkiseen IP-osoite

1. Luo virtuaalisia verkon aliverkon.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Luo Azure julkiseen IP-osoite (PIP) edusta IP-osoitevarannon resursseja.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Kuormituksen käyttää toimialueen nimi, julkiseen IP etuliite sen FQDN. Tässä esimerkissä täydelliset toimialuenimet ovat *lbnrpipv4.westus.cloudapp.azure.com* ja *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Luo FEP IP-määrityksiä ja taustatietokantaan osoite resurssivarantoon

1. Luo edusta osoite määritys, joka käyttää luomaasi julkiseen IP-osoitteita.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Luoda taustatietokantaan osoite.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Luo kg säännöt, NAT säännöt, näytteenottimen ja kuormituksen tasauspalvelun

Tässä esimerkissä luo seuraavat kohteet:

- Voit kääntää kaikki saapuvan liikenteen porttiin 443 porttiin 4443 NAT säännön
- kuormituksen tasauspalvelun sääntö, joka täsmää kaikki saapuvan liikenteen porttiin 80 taustatietokantaan resurssivarantoon osoitteet 80 porttiin.
- kuormituksen tasauspalvelun sääntö, joka sallii porttia 3389 VMs RDP-yhteyden.
- näytteenottimen sääntö, joka sivulla kunto-tilan tarkistaminen nimeltä *HealthProbe.aspx* tai palvelun porttiin 8080
- kuormituksen, joka käyttää nämä objektit

1. Luo NAT-sääntöjä.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Luo kunto näytteenottimen. Määrittäminen näytteenottimen kahdella tavalla:

    HTTP-näytteenotin

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    tai TCP näytteenottimen

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    Tässä esimerkissä tarkastellaan Käytä TCP-keräysputkien.

3. Kuormituksen tasauspalvelun säännön luominen

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Luo kuormituksen aiemmin luodun-objektien käyttäminen.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>Luo NIC taustatietokantaan VMs

1. Hanki VPN ja Virtual verkon aliverkon, jossa NIC on luoda.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Luo IP-määrityksiä ja NIC VMs.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Luo näennäiskoneiden ja määritä juuri luomasi NIC

Katso lisätietoja luomisesta AM [Luo ja määritä Windows-Virtual Machine Resurssienhallinta ja PowerShellin Azure](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Saatavuus ja tallennustilaa tilin luominen

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Luo kullekin AM ja Määritä edellisen luotu NIC

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-ps.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)
