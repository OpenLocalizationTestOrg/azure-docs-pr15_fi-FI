<properties
    pageTitle="Windows-AM kopion luominen | Microsoft Azure"
    description="Opettele luomaan kopion erityinen AM Azure Windows on resurssien hallinnan käyttöönottomalli."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Voit luoda AM erityinen Näennäiskiintolevyn

Luo uusi AM liittämällä erityinen Näennäiskiintolevyn kuin OS aseman PowerShellin avulla. Erityistä Näennäiskiintolevyn ylläpitää käyttäjätilit, sovellusten ja alkuperäinen AM tilan muut tiedot. 

Jos haluat luoda AM generalized Näennäiskiintolevyn, katso [luominen AM generalized Näennäiskiintolevyn kuvasta](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Luo aliverkon ja vNet

Luo vNet ja aliverkon [VPN](../virtual-network/virtual-networks-overview.md).

1. Luo aliverkon. Tässä esimerkissä Luo aliverkon nimeltä **mySubNet**resurssin ryhmän **myResourceGroup**, ja määrittää aliverkon osoite etuliite **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Luo vNet. Tässä esimerkissä **myVnetName**, sijainti, johon **Länsi US**ja **10.0.0.0/16**virtual verkon osoitteen etuliite VPN-nimi. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Luo julkinen IP-osoite ja NIC: ssä

Virtuaalikoneen virtual verkon tietoliikenteen käyttöön tarvitset [julkiseen IP-osoite](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ja Verkkokäyttöliittymän.

1. Luo julkinen IP. Tässä esimerkissä julkiseen IP-osoitteen nimi on määritetty **myIP**.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Luo NIC-sivustosta. Tässä esimerkissä NIC nimi on määritetty **myNicName**.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Verkko-käyttöoikeusryhmän ja RDP-säännön luominen

Jos haluat voi kirjautua oman AM käyttämällä RDP-on suojaussääntö, joka sallii RDP-tila käyttöön portti 3389. Koska uusi AM Näennäiskiintolevyn on luotu aiemmin luodun erityinen AM AM luomisen jälkeen käyttää lähde virtual tietokoneesta, kirjaudu sisään käyttämällä RDP oli aiemmin luotu tili.

Tässä esimerkissä NSG nimi **myNsg** ja **myRdpRule**RDP säännön nimi.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Saat lisätietoja päätepisteet ja NSG säännöt [avaaminen portit AM PowerShellin Azure-tietokannassa](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>Luo AM määritys

Liitä kopioitu Näennäiskiintolevy kuin OS Näennäiskiintolevyn määrittää AM-määritys.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Jos sinulla on tietoja levyjä, jotka on liitettävä AM, sinun kannattaa myös Lisää seuraava: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Käyttöjärjestelmä levyn URL-osoitteet ja tietojen näyttää suunnilleen tältä: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Voit etsiä tästä portaalissa selaamalla kohde tallennustilan säilö, valitsemalla käyttöjärjestelmän tai tietojen Näennäiskiintolevyn, joka on kopioitu ja kopioimalla sitten URL-Osoitteen sisällön.


## <a name="create-the-vm"></a>Luo AM

Luo käyttämällä määritykset, jonka juuri luonut AM.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Jos tämä komento onnistui, näet tulosteen tältä:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Varmista, AM luotiin 
 
Pitäisi näkyä juuri luomasi AM joko [Azure portal](https://portal.azure.com)-kohdassa **Selaa** > **näennäiskoneiden**tai käyttämällä PowerShell-komennot:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Seuraavat vaiheet

Kirjautuaksesi sisään uuden virtuaalikoneen [portal](https://portal.azure.com)AM selaamalla, **Yhdistä**ja avaa Remote Desktop RDP-tiedosto. Uusi virtuaalikoneen kirjautuminen alkuperäisen virtuaalikoneen tilin tunnistetiedot avulla. Lisätietoja on artikkelissa [liittämisestä ja Azure virtual-tietokoneessa, jossa Windows-kirjautumisen](virtual-machines-windows-connect-logon.md).







