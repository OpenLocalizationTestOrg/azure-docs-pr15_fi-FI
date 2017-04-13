<properties
   pageTitle="Luo sisäinen kuormituksen, resurssien hallinta PowerShellin avulla | Microsoft Azure"
   description="Opettele luomaan sisäinen kuormituksen, resurssien hallinta PowerShellin avulla"
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

# <a name="create-an-internal-load-balancer-using-powershell"></a>Luo sisäinen kuormituksen, PowerShellin avulla

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönottomalli](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Seuraavassa kerrotaan, kuinka luominen Azure resurssien hallinnan avulla PowerShellin sisäinen kuormituksen. Azure resurssien hallinnan avulla Luo sisäinen kuormituksen kohteet on määritetty yksitellen ja sitten yhdistetty kuormituksen tasauspalvelun.

Haluat luoda ja määrittää kuormituksen tasauspalvelun ottamaan seuraaville objekteille:

- Lopeta IP-määritys eteen - määritetään saapuvaa verkkoliikennettä yksityinen IP-osoite
- Taustajärjestelmä osoite resurssivarantoon - määritetään verkon liityntäkohdat, joihin saavat tulossa edusta IP varannon kuormitus tasataan liikenne
- Kuormituksen tasaamisen säännöt - lähde- ja kuormituksen paikallisen portin määritykset.
- Probes - määrittää kunto tila näytteenottimen virtuaalikoneen esiintymien.
- Saapuvan liikenteen säännöt NAT - määrittää portin säännöistä käyttää suoraan virtuaalikoneen esiintymät.

Saat lisätietoja kuormituksen tasauspalvelun osat Azure resurssien hallinnan osoitteessa [Azure Resurssienhallinta tuki kuormituksen](load-balancer-arm.md).

Seuraavassa kerrotaan, kuinka voit määrittää kuormituksen kaksi näennäiskoneiden välillä.


## <a name="setup-powershell-to-use-resource-manager"></a>PowerShellin käyttäminen Resurssienhallinta asetukset

Varmista, että tuotannon Azure-moduulin uusimman PowerShell- ja PowerShell-asetukset oikein Azure-tilauksen.

### <a name="step-1"></a>Vaihe 1

        Login-AzureRmAccount

### <a name="step-2"></a>Vaihe 2

Tarkista tilin tilaukset

        Get-AzureRmSubscription

Voit pyydetään todentaa ja tunnistetiedot.<BR>

### <a name="step-3"></a>Vaihe 3

Valitse, mitä Azure tilauksistasi käyttämään. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Luo resurssiryhmä kuormituksen

### <a name="step-4"></a>Vaihe 4

Luo uusi resurssiryhmä (ohita tämä vaihe aiemmin resurssiryhmä käytettäessä)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät, valitse sijainti. Tämä käytetään oletussijainti resurssien resurssin kyseisen ryhmän. Varmista, että kaikki komennot luomiseen kuormituksen käyttää samaa resurssiryhmä.

Yllä olevassa esimerkissä luomaasi nimeltä "NRP RG" ja "Länsi US" sijainnin resurssiryhmä.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Luo VPN ja edusta IP-ryhmän yksityinen IP-osoite


### <a name="step-1"></a>Vaihe 1

Luo virtuaalisia verkon aliverkon ja määrittää muuttujan $backendSubnet

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Luo virtuaalisia verkon:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Luo virtuaalisia verkon ja lisää aliverkon kg voidaan aliverkon virtual verkon NRPVNet ja määrittää muuttujan $vnet



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Eteen loppu IP resurssivarantoon ja Taustajärjestelmä osoite resurssivarannon luominen

Edusta-IP-sovellussarjan määrittäminen, saapuvat kuormituksen tasauspalvelun verkon liikenne ja Taustajärjestelmä osoitevarannon vastaanottamaan kuormituksen saapuva liikenne.

### <a name="step-1"></a>Vaihe 1

Yksityisen IP-osoitteen 10.0.2.5 käytön aliverkon-10.0.2.0/24 joka saapuvien verkon liikenne päätepistettä edusta IP-ryhmän luominen

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>Vaihe 2

Määritä saapuvan liikenteen vastaanottaa edusta IP varannon uudelleen osoite resurssivarantoon:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Luo kg sääntöjä, NAT sääntöjä, näytteenottimen ja kuormituksen

Kun olet luonut edusta-IP-ryhmän ja Taustajärjestelmä osoite resurssivarantoon, tarvitset Luo säännöt, jotka kuuluvat kuormituksen tasauspalvelun resurssi:

### <a name="step-1"></a>Vaihe 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Yllä olevassa esimerkissä luodaan seuraavat kohteet:

- NAT-sääntö, joka kaiken saapuvan liikenteen porttiin 3441 siirtyvät portti 3389.
- toisen NAT-säännön joka kaiken saapuvan liikenteen porttiin 3442 siirtyvät portti 3389.
- kuormituksen tasauspalvelun sääntöön, jossa Lataa saldo kaikki saapuvan liikenteen julkisen portin 80 paikallisen porttia 80 uudelleen osoite sarjassa.
- näytteenottimen sääntöön, jossa tarkistaa polku "HealthProbe.aspx" kunto tila



### <a name="step-2"></a>Vaihe 2

Luo kuormituksen laskemalla yhteen kaikki objektit (NAT sääntöjä, kuormituksen tasauspalvelun sääntöjä, näytteenottimen määrityksiä):

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Luo verkon liityntäkohdat

Luotuasi sisäinen kuormituksen on määrittää, mitkä verkkoliittymät vastaanottaminen saapuvien kuormitus tasataan verkkoliikennettä, NAT säännöt ja näytteenottimen. Verkkoliittymän tässä tapauksessa on määritetty yksitellen ja voi määrittää virtual machine myöhemmin.


### <a name="step-1"></a>Vaihe 1


Hae resurssin virtual verkko- ja aliverkon verkkoliittymät luomiseen:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Tämä vaihe luo verkko-käyttöliittymä, joka kuormituksen tasauspalvelun uudelleen ryhmään kuuluvat ja liittää NAT ensimmäistä sääntöä, RDP verkko-liittymän:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Vaihe 2

Luo toinen verkkoliittymän kutsutaan Nic2 kg voi:

Tämä vaihe luo toinen verkkoliittymän, RDP luoda liittäminen samaan kuormituksen tasauspalvelun uudelleen resurssivarantoon ja liittämisestä NAT toisen säännön:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Lopputulos näytetään seuraavasti:

    $backendnic1

Odotettu tulos:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Vaihe 3

Lisää AzureRmVMNetworkInterface-komennon avulla voit määrittää Verkkokortti virtual koneeseen.

Voit etsiä vaiheittaiset ohjeet virtual machine luominen ja lisääminen NIC, seuraavat asiakirjat: [Luo Azure-AM PowerShellin avulla](../virtual-machines/virtual-machines-windows-ps-create.md).

Jos sinulla on jo luotu virtual koneen, voit lisätä Verkkokäyttöliittymän seuraavasti:

#### <a name="step-1"></a>Vaihe 1

Lataa kuormituksen tasauspalvelun resurssin muuttujan (Jos sinulla ole vielä). Käytetty muuttuja kutsutaan $lb ja samannimiset edellä luotu kuormituksen tasauspalvelun resurssista.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Vaihe 2

Lataa muuttujan Taustajärjestelmä-määritys.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Vaihe 3

Lataa aiemmin luotuja verkon-liittymän muuttujaksi. muuttujan nimi, jolla on $ NIC-sivustosta. Verkon käyttöliittymän nimi on sama kuin edellä olevassa esimerkissä.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Vaihe 4

Muuta verkko-liittymän Taustajärjestelmä-määrityksiä.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Vaihe 5

Tallenna verkko-liittymän objekti.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Kun verkkoliittymän on lisätty kuormituksen tasauspalvelun Taustajärjestelmä resurssivarantoon, se käynnistyy vastaanottaminen verkkoliikennettä kuormituksen kuormituksen tasauspalvelun resurssin sääntöjen perusteella.

## <a name="update-an-existing-load-balancer"></a>Päivitä aiemmin kuormituksen


### <a name="step-1"></a>Vaihe 1

Käytä edellä olevassa esimerkissä-kuormituksen, määrittää kuormituksen tasauspalvelun objektin muuttujan käyttämällä Hae AzureRmLoadBalancer $slb

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Vaihe 2

Seuraavassa esimerkissä lisäät uuden saapuva NAT säännön käyttämällä portin 81 edustan ja portin 8181 uudelleen ryhmän aiemmin kuormituksen

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Vaihe 3

Tallenna uusi konfigurointi käyttämällä määrittäminen AzureLoadBalancer

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Poista kuormituksen tasaus

Poista AzureRmLoadBalancer-komennon avulla voit poistaa aiemmin luodun kuormituksen nimeltä "NRP-kg" resurssin ryhmän nimeltä "NRP RG"

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Voit käyttää valinnainen vaihtaminen - voimassa poistettavaksi kehotteen välttämiseksi.



## <a name="next-steps"></a>Seuraavat vaiheet

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)