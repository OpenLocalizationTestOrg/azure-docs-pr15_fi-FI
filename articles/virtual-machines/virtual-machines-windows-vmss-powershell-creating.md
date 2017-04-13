<properties
    pageTitle="Virtuaalikoneen asteikko joukkojen PowerShell cmdlet-komentojen käyttäminen luominen | Microsoft Azure"
    description="Aloita luomisen ja ylläpidon oman ensimmäisen Azure virtuaalikoneen asteikko joukkoja Azure PowerShellin cmdlet-komennot"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="danielsollondon"/>

# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>Virtuaalikoneen asteikko joukkojen PowerShell cmdlet-komentojen käyttäminen luominen

Seuraavassa on esimerkki siitä, miten voit luoda virtuaalikoneen asteikko-Set(VMSS), se luo VMSS 3 solmujen kaikki liittyvät verkko ja tallennustilaa.

## <a name="first-steps"></a>Ensimmäiset vaiheet
Varmista, sinulla on asennettu uusimmat PowerShellin Azure-moduulissa, tämä sisältää tarvitaan ylläpitää ja luoda VMSS PowerShell-komentosovelmat.
Komentorivi Työkalut-valikosta [tähän](http://aka.ms/webpi-azps) uusinta saatavilla Azure-moduuleissa.

Voit etsiä VMSS liittyvät komentosovelmat, käyttää etsintämerkkijonon \*VMSS\*.

## <a name="creating-a-vmss"></a>VMSS luominen

##### <a name="create-resource-group"></a>Luo resurssiryhmä

```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

##### <a name="create-storage-account"></a>Tallennustilan tilin luominen

Määritä tili tallennustyyppi / nimi.

```
$stoname = 'sto' + $rgname;
$stotype = 'Standard_LRS';
 New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname -Location $loc -SkuName $stotype -Kind "Storage";

$stoaccount = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname;
```

#### <a name="create-networking-vnet--subnet"></a>Luo verkko (VNET / aliverkon)

##### <a name="subnet-specification"></a>Aliverkon määritys

```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

##### <a name="vnet-specification"></a>VNET määritys

```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

#In this case below we assume the new subnet is the only one, note difference if you have one already or have adjusted this code to more than one subnet.
$subnetId = $vnet.Subnets[0].Id;
```

##### <a name="create-public-ip-resource-to-allow-external-access"></a>Luo julkinen IP resurssin ulkoisen käytön salliminen

Tämä sidottava kuormituksen avulla.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

##### <a name="create-and-configure-load-balancer"></a>Luo ja Määritä kuormituksen

```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

#Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

##### <a name="configure-load-balancer"></a>Kuormituksen tasauspalvelun asetusten määrittäminen
Luo Taustajärjestelmä osoite resurssivarantoon Config, tämä jaetaan VMs-VMSS NIC.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Lataa tasapainoinen näytteenottimen portin määrittäminen, muuta asetuksia haluamallasi tavalla sovelluksen.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Luo NAT säännöt suora reititetty yhteys (kuormitus ei tasataan) VMSS kautta ladata tasaustoiminto Huomautus Tämä esitellään käyttämällä RDP-VMs, tämä koskee vain esittely ja sisäisen VNET menetelmiä käytetään RDP'ing seuraavissa palvelimissa.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Luo ladata saapuva valintasääntö, tässä esimerkissä näkyy ladata Vastatilin portti 80 pyynnöt, käyttäen aiemmissa vaiheissa asetuksia.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Voit luoda kuormituksen määritys.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Tarkista kg, Tarkista kuormitus tasataan portin kokoonpanomääritysten yhteydessä, Huomaa, et näe NAT saapuvan liikenteen säännöt, kunnes AM-VMSS luodaan.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-vmss"></a>Määritä ja luo VMSS

Huomautus-infrastruktuurin tässä esimerkissä näytetään, miten asennuksen jakaminen ja skaalata web liikenne yli VMSS, mutta tässä määritetty VMs kuvat ei ole asennettu web palvelut.

```
#specify VMSS Name
$vmssName = 'vmss' + $rgname;

##specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

$vhdContainer = "https://" + $stoname + ".blob.core.windows.net/" + $vmssName;

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

Kuormituksen ja aliverkon NIC sitoa

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

Luo VMSS Config

```
#Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
  	| Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
  	| Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
  	| Set-AzureRmVmssStorageProfile -Name 'test' -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName -VhdContainer $vhdContainer `
  	| Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

Muodosta VMSS Config

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Kun olet luonut VMSS. Voit testata yhteyden yksittäisiä AM käyttämällä RDP tässä esimerkissä:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
