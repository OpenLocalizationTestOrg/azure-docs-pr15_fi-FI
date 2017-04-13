<properties
    pageTitle="Muuta joukon VMs käytettävyys | Microsoft Azure"
    description="Lue, miten voit muuttaa käytettävyyttä määrittäminen oman näennäiskoneiden PowerShellin Azure ja resurssien hallinnan käyttöönottomalli."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="drewm"/>



# <a name="change-the-availability-set-for-a-windows-vm"></a>Windows-AM määrittäminen käytettävyyden muuttaminen

Seuraavat vaiheet kerrotaan, kuinka voit vaihtaa käytettävyys AM, Azure PowerShellin avulla. AM voidaan lisätä vain kun se on luotu käyttöön. Jotta voit muuttaa käytettävyyttä määrittäminen, sinun täytyy poistaa ja luo virtuaalikoneen. 

## <a name="change-the-availability-set-using-powershell"></a>Määritä PowerShellin käytettävyyden muuttaminen

1. Sieppaa muokattava AM avaimen seuraavat tiedot.

    Nimi AM
    
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
 
    AM kokoa
    
    ```powershell
    $vm.HardwareProfile.VmSize
    ```

    Verkon ensisijaisen verkkoliittymän ja valinnainen verkko-rajapintojen löytämääsi AM
    
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```

    OS levyn profiili

    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```

    Levy-profiileista kunkin tiedot-levyllä 
    
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```

    AM laajennuksia. 
    
    ```powershell
    $vm.Extensions
    ```

2. Poista AM poistamatta levyjen tai Verkkoliittymät.

    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```

3. Jos sitä ei ole jo käytettävyys luominen

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```

4. Luo uusi käytettävyys joukon käyttäminen AM

    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>

    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]

    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 

    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 

5. Lisää tietoja levyille ja tunnisteet. Lisätietoja on artikkelissa [Liittää tietoja levyn AM](virtual-machines-windows-attach-disk-portal.md) ja [Tunniste määritysten mallit](virtual-machines-windows-extensions-configuration-samples.md). Tietoja levyjen ja laajennukset voidaan lisätä PowerShell tai Azure CLI AM.

## <a name="example-script"></a>Esimerkkikomentosarja

Seuraavaa komentosarjaa on esimerkki kerännyt tarvittavat tiedot, poistaa alkuperäisen AM ja luotaessa se käytettävyys-ryhmässä.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisää tallennustilaa oman AM lisäämällä muita [tietoja levy](virtual-machines-windows-attach-disk-portal.md).

