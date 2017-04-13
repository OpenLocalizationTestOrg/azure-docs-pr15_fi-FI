<properties
    pageTitle="Voit luoda AM generalized Näennäiskiintolevyn | Microsoft Azure"
    description="Opettele luomaan Windows-virtual machine generalized Näennäiskiintolevyn kuvasta PowerShellin Azure käyttäminen resurssien hallinnan käyttöönottomalli."
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
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Luo AM generalized Näennäiskiintolevyn kuvasta

Generalized Näennäiskiintolevyn kuva on ollut kaikki henkilökohtaiset tietosi poistetaan [Sysprepin](virtual-machines-windows-generalize-vhd.md)avulla. Voit luoda generalized Näennäiskiintolevyn suorittamalla Sysprep paikallisen-AM, valitse [lataamisesta, Azure Näennäiskiintolevyn](virtual-machines-windows-upload-image.md), tai suorittamalla Sysprep aiemmin luodun Azure AM ja [kopioimalla Näennäiskiintolevyn](virtual-machines-windows-vhd-copy.md).

Voit halutessasi AM luominen erityinen Näennäiskiintolevyn artikkelissa [luominen AM-erityinen Näennäiskiintolevyn](virtual-machines-windows-create-vm-specialized.md).

Nopein tapa luoda AM generalized Näennäiskiintolevyn on käyttää [pikaopas malli] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Edellytykset

Jos aiot käyttää Näennäiskiintolevyn ladattuja paikallisen AM, kuten luotua käyttämällä Hyper-V, varmista, että olet toiminut [valmisteleminen Windows Azure lataaminen Näennäiskiintolevyn](virtual-machines-windows-prepare-for-upload-vhd-image.md)ohjeita. 

Ladatun näennäiskiintolevyjen ja aiemmin Azure AM näennäiskiintolevyjen on generalized, ennen kuin voit luoda AM tällä menetelmällä. Lisätietoja on artikkelissa [Yleistä Windows-virtual machine Sysprepin avulla](virtual-machines-windows-generalize-vhd.md). 


## <a name="set-the-uri-of-the-vhd"></a>Määritä Näennäiskiintolevyn URI

Muoto on käytettävä Näennäiskiintolevyn URI: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. Tässä esimerkissä nimeltä **myVHD** Näennäiskiintolevyn on-tallennustilan tilin **mystorageaccount** -säilö **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Luo virtuaalisia verkko

Luo vNet ja aliverkon [VPN](../virtual-network/virtual-networks-overview.md).


1. Luo aliverkon. Seuraava esimerkki luo **mySubnet** resurssin ryhmän **myResourceGroup** **10.0.0.0/24**osoitteen etuliite-niminen aliverkon.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Luo virtuaalisia verkkoon. Seuraava esimerkki luo virtual verkon nimeltä **myVnet** **10.0.0.0/16** **Länsi US** sijainnissa osoite etuliitteellä.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Luo julkinen IP-osoite ja verkon liittymän

Virtuaalikoneen virtual verkon tietoliikenteen käyttöön tarvitset [julkiseen IP-osoite](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ja Verkkokäyttöliittymän.

1. Luo julkinen IP-osoite. Tässä esimerkissä luodaan nimeltä **myPip**julkiseen IP-osoite. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Luo NIC-sivustosta. Tässä esimerkissä luodaan NIC, jonka nimi on **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Verkko-käyttöoikeusryhmän ja RDP-säännön luominen

Jos haluat voi kirjautua oman AM käyttämällä RDP-suojauksen sääntö, joka sallii RDP-tila käyttöön portti 3389 on. 

Tässä esimerkissä luodaan NSG nimeltä **myNsg** , joka sisältää säännön kutsutaan **myRdpRule** , joka sallii RDP liikenne portin 3389 kautta. Saat lisätietoja NSGs [avaaminen portit AM PowerShellin Azure-tietokannassa](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Luo muuttujan virtual verkossa

Luo muuttujan valmiit virtual verkossa. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>Luo AM

Seuraavaa PowerShell-komentosarjaa näyttää virtuaalikoneen käyttömahdollisuudet määrittäminen ja käyttäminen ladattujen AM kuva lähteenä uusi asennus.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Varmista, AM luotiin 

Kun olet valmis, näkyviin tulee juuri luomasi AM [Azure portal](https://portal.azure.com) -kohdassa **Selaa** > **näennäiskoneiden**tai käyttämällä PowerShell-komennot:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Seuraavat vaiheet

Hallinnasta uusi virtuaalikoneen Azure PowerShellin avulla on artikkelissa [Hallitse näennäiskoneiden Azure Resurssienhallinta ja PowerShellin avulla](virtual-machines-windows-ps-manage.md).


