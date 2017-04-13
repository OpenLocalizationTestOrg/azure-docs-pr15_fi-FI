<properties
   pageTitle="Luo Windows AM useita NIC | Microsoft Azure"
   description="Opi luomaan Windows AM sisältää useita NIC liitetään PowerShellin Azure tai Resurssienhallinta mallien avulla."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Windows-AM luomalla useita NIC
Voit luoda virtual machine (AM) Azure-tietokannassa, jossa on useita VPN-liittymät (NIC) liitetään. Käytetty vaihtoehto olisi on eri aliverkosta edusta- ja yhteyden tai verkon omistautunut seurantaa tai varmuuskopio-ratkaisun. Tässä artikkelissa on luomalla AM liitetty useita NIC Pikakomennot. Lisätietoja on myös useita NIC sisällä oman PowerShell-komentosarjojen luomisesta lukea lisää tietoja [usean NIC VMs käyttöönotto](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Eri [AM koot](virtual-machines-windows-sizes.md) Tukipuhelinnumero erilaisten NIC-kokoa niin että AM vastaavasti.

>[AZURE.WARNING] Sinun on liitettävä useita NIC, kun luot AM - NIC ei voi lisätä aiemmin AM. Voit [luoda AM, joka perustuu alkuperäiseen virtual näiden levyjen asemasta](virtual-machines-windows-vhd-copy.md) ja luoda useita NIC, kun otat käyttöön AM.

## <a name="create-core-resources"></a>Luo core resurssit
Varmista, että sinulla on [uusin PowerShellin Azure asentanut ja määrittänyt](../powershell-install-configure.md). Kirjautuminen Azure-tili:

```powershell
Login-AzureRmAccount
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ohjelmistopakettia `myResourceGroup`, `mystorageaccount`, ja `myVM`.

Luo resurssiryhmä. Seuraavassa esimerkissä luodaan nimeltä resurssiryhmä `myResourceGroup` - `WestUs` sijainti:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Tallennustilan tilin pitoon oman VMs luominen. Seuraavassa esimerkissä luodaan tallennustilan-tili nimeltä `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Luo virtuaalisia verkko- ja aliverkosta
Määrittää kahden VPN - aliverkon yksi edusta liikenteen ja yksi taustatietokantaan tietoliikenteen. Seuraava esimerkki määrittää kahden aliverkon-niminen `mySubnetFrontEnd` ja `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Luo virtuaalisia verkko- ja aliverkosta. Seuraavassa esimerkissä luodaan virtual nimeltä verkon `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Luo useita NIC
Luo kaksi NIC-yksi Verkkokortti edusta aliverkon, ja yhden NIC liittäminen taustatietokantaan aliverkon. Seuraavassa esimerkissä luodaan kahden NIC-niminen `myNic1` ja `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Yleensä voit myös luoda [verkon käyttöoikeusryhmän](../virtual-network/virtual-networks-nsg.md) tai hallita ja jakaa oman VMs liikenne [kuormituksen](../load-balancer/load-balancer-overview.md) . [Lisätietoja usean NIC AM](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) artikkelin opastaa verkon käyttöoikeusryhmän luominen ja määrittäminen NIC.


## <a name="create-the-virtual-machine"></a>Luo virtuaalikoneen
Nyt luonnissa AM-määritys. Jokaisen AM koon on rajoitukset, voit lisätä AM NIC kokonaismäärän. Lisätietoja [Windowsin AM koot](virtual-machines-windows-sizes.md). 

Määritä ensin AM tunnistetiedot `$cred` muuttujan seuraavasti:

```powershell
$cred = Get-Credential
```

Seuraava esimerkki määrittää AM, jonka nimi on `myVM` ja käyttää AM koko, joka tukee jopa kaksi NIC (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Luo AM config loput. Seuraavassa esimerkissä luodaan Windows Server 2012 R2 AM:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Liitä aiemmin luotu kaksi NIC:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Määrittää uuden AM virtual levyn ja tallennustilaa seuraavasti:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Luo-AM:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Luominen useita NIC Resurssienhallinta mallien käyttäminen
Azure Resurssienhallinta malleja määritettäviä JSON-tiedostojen avulla voit määrittää ympäristösi. Voit lukea [Azure resurssien hallinnan yleiskatsaus](../azure-resource-manager/resource-group-overview.md). Resurssienhallinta mallit avulla voit luoda useita kertoja resurssin käyttöönoton, kuten luominen useita NIC aikana. *Kopioi* avulla voit määrittää minuuttimäärän, jonka esiintymät luominen:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Lisätietoja [luominen *kopioimalla*useita kertoja](../resource-group-create-multiple.md). 

Voit myös käyttää `copyIndex()` liittäminen luvun sitten resurssinimi, jonka avulla voit luoda `myNic1`, `MyNic2`jne. Seuraavassa kuvassa näkyy esimerkki liität indeksiarvo:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Voit lukea täydellinen esimerkki [luominen useita NIC Resurssienhallinta mallien avulla](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Seuraavat vaiheet
Varmista, että voit tarkastella [Windowsin AM koot](virtual-machines-windows-sizes.md) yritettäessä luomalla AM useita NIC. Kiinnitä huomiota NIC jokaisen AM koon tukee enimmäismäärä. 

Muista, että et voi lisätä uusia NIC aiemmin AM, sinun on luotava kaikki NIC, kun otat käyttöön AM. Huolehtia suunnittelun varmistaaksesi, että kaikki tarvittavat verkkoyhteys sen-käyttöönoton yhteydessä.