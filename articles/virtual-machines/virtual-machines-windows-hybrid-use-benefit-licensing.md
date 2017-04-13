<properties
   pageTitle="Ikkunan Server Azure Hybrid Käytä hyötyä | Microsoft Azure"
   description="Lue, miten voit suurentaa tuoda paikallisen käyttöoikeuksien Azure Windows Server SA-ylläpito etuja"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Windows Server Azure Hybrid Käytä etu

Windows Server käyttäminen Software Assurance-asiakkaiden tuo oman paikallista Azure Windows Server-käyttöoikeudet ja suorita Windows Server VMs Azure rajoitetun kustannus. Azure Hybrid Käytä etu avulla voit suorittaa Windows Server VMs Azure ja Hae laskuttaa vain perus Laske korko. Lisätietoja artikkelissa [Azure Hybrid Käytä etu käyttöoikeudet sivun](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Tässä artikkelissa kerrotaan asentamisesta Windows Server VMs käyttämään tämän Käyttöoikeussopimuksen edun Azure-tietokannassa.

> [AZURE.NOTE] Ottaa käyttöön Windows Server VMs käyttävien Azure Hybrid käyttää etu, et voi käyttää Azure Marketplace-kuvat. Sinun on otettava käyttöön oman VMs PowerShell tai Resurssienhallinta mallien avulla voit rekisteröidä oikein oman VMs oikeutettu perus Laske korko alennus.

## <a name="pre-requisites"></a>Vaatimukset
Jotta voit hyödyntää Azure Hybrid Käytä etu, Windows Server VMs Azure-tietokannassa on muutama vaatimukset:

- On asennettu PowerShellin Azure-moduuli
- Windows Server-Näennäiskiintolevyn on ladattu Azure säilöön

### <a name="install-azure-powershell"></a>Azure PowerShellin asentaminen
Varmista, että olet [asentanut ja määrittänyt uusimman PowerShellin Azure](../powershell-install-configure.md). Myös silloin, kun aiot ottaa yhteyttä VMs Resurssienhallinta mallien avulla, sinun on edelleen asennettuna, voit ladata Windows Server-Näennäiskiintolevyn PowerShellin Azure (katso seuraava vaihe).

### <a name="upload-a-windows-server-vhd"></a>Lataa Windows Server Näennäiskiintolevyn

Ottaa käyttöön Windows Server-AM Azure-tietokannassa, sinun on Näennäiskiintolevyn, joka sisältää perus Windows Server-muodosta luominen. Tämä Näennäiskiintolevyn on otettu valmisteltava Sysprep ennen lataaminen Azure. Voit [lukea lisää Näennäiskiintolevyn vaatimukset ja Sysprepin prosessi](./virtual-machines-windows-upload-image.md) ja [Palvelinrooleista Sysprep-tuki](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Varmuuskopioi AM ennen Sysprepin suorittamista. Kun olet valmistellut oman Näennäiskiintolevyn, että Azure-tallennustilan tilin käyttämällä Näennäiskiintolevyn lataaminen `Add-AzureRmVhd` cmdlet-komento, toimimalla seuraavasti:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Serverin ja SharePoint Serverin Dynamics voit myös hyödyntää Software Assurance-käyttöoikeudet. Haluat asentaa sovelluksen osia ja antaa käyttöoikeuden näppäimet vastaavasti ja sitten levyn näköistiedoston lataaminen Azure Windows Server-kuvan valmisteleminen. Tarkista asianmukaiset käytetään Sysprep sovelluksen, kuten [SQL Serverin asentaminen Huomioitavaa Sysprepin avulla](https://msdn.microsoft.com/library/ee210754.aspx) tai [luominen SharePoint Server 2016 viittaus kuva (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

Voit myös lukea lisää tietoja [ladataan Näennäiskiintolevyn Azure-prosessi](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Tässä artikkelissa kerrotaan Windows Server VMs käyttöönotto. Voit myös ottaa Windows asiakkaan VMs samalla tavalla. Seuraavissa esimerkeissä korvaat `Server` kanssa `Client` asianmukaisesti.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Ottaa käyttöön AM kautta PowerShell Pika-aloitus
Kun otat Windows Server-AM PowerShellin kautta, on lisäparametri varten `-LicenseType`. Kun olet oman Näennäiskiintolevyn Azure ladataan, Luo uusi AM käyttäen `New-AzureRmVM` ja määritä käyttöoikeudet tyyppi seuraavasti:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Voit [lukea lisää vaiheittainen AM Azure-tietokannassa PowerShellin kautta ottamisesta käyttöön](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) alla tai lue enemmän kuvailevaa opas eri ohjeiden avulla voit [luoda Windows-AM Resurssienhallinta ja PowerShellin avulla](./virtual-machines-windows-ps-create.md).

## <a name="deploy-a-vm-via-resource-manager"></a>Ottaa käyttöön AM kautta resurssien hallinta
Resurssienhallinta-mallit, lisäparametri sisällä `licenseType` voidaan määrittää. Voit lukea lisää tietoja [authoring Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md). Kun oman Näennäiskiintolevyn Azure ladataan, Muokkaa voit jättää käyttöoikeustyyppi Laske-palvelun ja otetaan käyttöön mallin normaalisti Resurssienhallinta mallia:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Varmista, että AM on käyttävien käyttöoikeuksien myöntämistä etu
Kun olet ottanut oman AM joko PowerShell- tai resurssien hallinnan käyttöönottomenetelmä kautta, tarkista käyttöoikeustyyppi kanssa `Get-AzureRmVM` seuraavasti:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Tulos on seuraavanlainen:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Tämä kontrastin seuraavat AM ilman Azure Hybrid Käytä etu käyttöoikeudet, kuten AM, suoraan Azure valikoimasta käyttöön:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>PowerShellin käytön vaiheita kuvataan

Yksityiskohtainen PowerShell-ohjeita Näytä AM kokonaisuudessaan käyttöön. Voit lukea lisää konteksti todellinen cmdlet-komennot ja eri osien aikaisemmassa [luominen Windows-AM Resurssienhallinta ja PowerShellin avulla](./virtual-machines-windows-ps-create.md). Käy luominen resurssiryhmä ja tallennustilaa tili ja virtual verkko-ja valitse Määritä oman AM ja luo lopuksi oman AM.
 
Ensin suojatusti hankkia tunnistetiedot, Määritä sijainti ja resurssiryhmän nimi:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Luo julkinen IP:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Määritä aliverkon, NIC ja VNET:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Nimen lisääminen AM ja luo AM config:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Määritä lisääminen OS:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Lisää oman NIC AM:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Määritä käytettävä tallennuskiintiön tili:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Lähetä SITEN, asianmukaisesti valmisteltu ja liitä oman AM käyttöä varten:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Lopuksi luoda oman AM ja määritä käyttöoikeudet käyttämiseksi Azure Hybrid Käytä etu:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Azure Hybrid Käytä etu käyttöoikeudet](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Lisätietoja [Resurssienhallinta mallien avulla](../azure-resource-manager/resource-group-overview.md).
