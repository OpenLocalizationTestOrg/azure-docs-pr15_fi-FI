<properties
   pageTitle="Luo Internetiin yhteydessä oleva-kuormituksen resurssien hallinta PowerShellin avulla | Microsoft Azure"
   description="Opettele luomaan Internetiin yhteydessä oleva-kuormituksen resurssien hallinta PowerShellin avulla"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="get-started-article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
   ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="get-started"></a>Luominen Internetiin yhteydessä oleva-kuormituksen resurssien hallinta PowerShellin avulla

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [luominen Internetiin yhteydessä oleva-kuormituksen perinteinen käyttöönotto-mallin avulla](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Ratkaisun käyttöönotto Azure PowerShell-toiminnolla

Seuraavien kerrotaan, kuinka voit luoda Internetiin yhteydessä oleva-kuormituksen PowerShellin Azure Resurssienhallinta. Azure resurssien hallinnan avulla kullekin resurssille on luotu ja määritetty erikseen ja ladata yhteen, kun haluat luoda kuormituksen tasauspalvelun.

Luo ja seuraaville objekteille ottamaan kuormituksen tasauspalvelun asetusten määrittäminen:

- Edustatietokannan IP-asetuksia: sisältää julkisten IP (PIP)-osoitteiden saapuvaa verkkoliikennettä.
- Taustatietokannan osoite resurssivarantoon: sisältää näennäiskoneiden vastaanottamaan verkkoliikennettä kuormituksen verkkoliittymät (NIC).
- Kuormituksen tasaamisen sääntöjä: sisältää sääntöjä, jotka vastaavat julkisen porttiin kuormituksen taustatietokantaan osoite resurssivarantoon porttiin.
- Saapuvan liikenteen säännöt NAT: sisältää sääntöjä, jotka vastaavat tiettyä virtual konetta taustatietokantaan osoite varannon portin julkisen kuormituksen-porttiin.
- Keräysputkien: sisältää kunto keräysputkien käytettävä virtuaalikoneen esiintymien taustatietokantaan osoite varannon käytettävyyden tarkistaminen.

Lisätietoja on artikkelissa [Azure Resurssienhallinta kuormituksen tuki](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Määritä PowerShell käyttämään resurssien hallinta

Varmista, että sinulla on Azure Resurssienhallinta-moduulin uusimman tuotannon PowerShell:

1. Azure kirjautuminen.

        Login-AzureRmAccount

    Anna tunnistetiedot pyydettäessä.

2. Tarkista tilaukset-tilin.

        Get-AzureRmSubscription

3. Valitse, mitä Azure tilauksistasi käyttämään.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Luo resurssiryhmä. (Ohita tämä vaihe, jos käytät aiemmin resurssiryhmä.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Luo virtuaalisia verkko- ja edusta IP-ryhmän julkiseen IP-osoite

1. Luo aliverkon ja virtual verkkoon.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Luo Azure julkisen IP address yritysresurssi, nimeltä **PublicIP**, käytettävän edusta IP-ryhmän kanssa DNS nimi **loadbalancernrp.westus.cloudapp.azure.com**. Seuraava komento käyttää staattinen kohdistustyyppi.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Kuormituksen käyttää toimialueen nimi, julkiseen IP etuliitteenä sen FQDN. Tämä on erilainen kuin perinteinen käyttöönotto-mallista, joka käyttää pilvipalvelussa sijaitsevaan kuormituksen täydellinen toimialuenimi.
    >Tässä esimerkissä FQDN on **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Edustatietokannan IP-ryhmän ja taustatietokantaan osoiteryhmän luominen

1. Luo edusta IP-ryhmän nimi, joka käyttää **PublicIp** resurssin **Kg Frontend** .

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Luo taustatietokantaan osoite resurssivarantoon nimeltä **kg Taustajärjestelmä**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Luo NAT sääntöjä, kuormituksen tasauspalvelun säännön, näytteenottimen ja kuormituksen tasauspalvelun

Tässä esimerkissä luo seuraavat kohteet:

- Voit kääntää kaikki saapuvan liikenteen porttiin 3441 porttiin 3389 NAT säännön
- Voit kääntää kaikki saapuvan liikenteen porttiin 3442 porttiin 3389 NAT säännön
- Näytteenottimen sääntö, joka sivulla kunto-tilan tarkistaminen nimeltä **HealthProbe.aspx**
- Kuormituksen tasauspalvelun sääntö, joka täsmää kaikki saapuvan liikenteen porttiin 80 taustatietokantaan resurssivarantoon osoitteet 80 porttiin
- Kuormituksen, joka käyttää nämä objektit

Noudata seuraavia ohjeita:

1. Luo NAT-sääntöjä.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Luo kunto näytteenottimen. Määrittäminen näytteenottimen kahdella tavalla:

    HTTP-näytteenotin

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    TCP näytteenottimen

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Kuormituksen tasauspalvelun säännön luominen

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Luo kuormituksen käyttämällä aiemmin luotuja objekteja.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>Luo NIC

Luo verkkoliittymät (tai muokata aiemmin luotuja) ja liitä ne NAT sääntöjä, kuormituksen tasauspalvelun säännöt ja näytteenottimet:

1. Hanki virtual verkko- ja VPN-aliverkon, silloin NIC tarvitse luoda.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Luo NIC, jonka nimi on **nic1 kg on**, ja liittää sen ensimmäinen NAT-sääntö ja ensimmäisen (ja vain) taustatietokantaan osoite resurssivarantoon.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Luo NIC, jonka nimi on **nic2 kg on**, ja liittää sen NAT toisen säännön ja ensimmäisen (ja vain) taustatietokantaan osoite resurssivarantoon.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Tarkista, NIC.

        $backendnic1

    Odotettu tulos:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Käytä `Add-AzureRmVMNetworkInterface` cmdlet NIC liittäminen eri VMs.

## <a name="create-a-virtual-machine"></a>Luo virtual machine

Ohjeita virtual machine luominen ja määrittäminen NIC: ssä on artikkelissa [Azure-AM Luo PowerShellin avulla](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Lisää verkkoliittymän kuormituksen

1. Noutaa kuormituksen Azure.

    Lataa kuormituksen tasauspalvelun resurssin muuttujan (Jos sinulla ole vielä). Muuttujan kutsutaan **$lb**. Käytä samoja nimiä kuormituksen tasauspalvelun resurssin aiemmin luomasi.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Lataa muuttujan taustatietokantaan-määritys.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. Lataa aiemmin luotuja verkon-liittymän muuttujaksi. Muuttujan nimi on **$nic**. Verkon käyttöliittymän nimi on sama kuin aiemmassa esimerkissä käytetty mallitietokanta.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. Muuta verkko-liittymän taustatietokantaan-määrityksiä.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Tallenna verkko-liittymän objekti.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Kun verkkoliittymän on lisätty kuormituksen tasauspalvelun taustatietokantaan resurssivarantoon, se käynnistyy vastaanottaminen verkkoliikennettä kuormituksen tasauspalvelun resurssin kuormituksen tasaamisen sääntöjen perusteella.

## <a name="update-an-existing-load-balancer"></a>Päivitä aiemmin kuormituksen

1. Käyttämällä kuormituksen aiemmissa esimerkiksi määrittää kuormituksen tasauspalvelun-objektin muuttujan **$slb** käyttämällä `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. Seuraavassa esimerkissä voit lisätä saapuvan liikenteen NAT-sääntö--käyttämällä portin 81 edusta varannon ja portin 8181 taustatietokantaan--resurssivarantoon aiemmin kuormituksen.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Tallenna uudet määritykset käyttämällä `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Poista kuormituksen tasaus

Käytä komentoa `Remove-AzureLoadBalancer` poistaa aiemmin luodun kuormituksen nimeltä **NRP kg** resurssiryhmä kutsutaan **NRP RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Voit käyttää valinnainen valitsin **-voimassa** välttämiseksi kehotteen poistamista varten.

## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-ps.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)
