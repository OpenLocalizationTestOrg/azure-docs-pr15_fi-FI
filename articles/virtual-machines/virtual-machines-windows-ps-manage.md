<properties
    pageTitle="Hallitse VMs Resurssienhallinta ja PowerShellin avulla | Microsoft Azure"
    description="Voit hallita näennäiskoneiden Azure Resurssienhallinta ja PowerShellin avulla."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Hallitse Azuren näennäiskoneiden Resurssienhallinta ja PowerShellin avulla

## <a name="install-azure-powershell"></a>Azure PowerShellin asentaminen
 
Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja PowerShellin Azure uusimman version asentamisesta, valitsemalla tilauksen ja tiliisi kirjautuminen.

## <a name="set-variables"></a>Määritä muuttujat

Kaikki komennot on artikkelissa edellyttävät resurssiryhmä, jossa virtuaalikoneen sijaitsee nimi ja hallita virtuaalikoneen nimi. Korvaa **$rgName** arvo, joka sisältää virtuaalikoneen resurssiryhmän nimi. Korvaa **$vmName** arvosta AM nimi. Luo muuttujat.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Näyttää tietoja virtual machine

Hanki virtuaalikoneen tiedot.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Se palauttaa suunnilleen tässä esimerkissä:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Lopeta virtual machine

Pysäyttää käynnissä virtuaalikoneen.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Sinua pyydetään vahvistamaan:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Kirjoita lopettavat virtuaalikoneen **Y** .

Muutaman minuutin kuluttua se palauttaa suunnilleen tässä esimerkissä:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Käynnistettäessä

Aloita virtuaalikoneen, jos se pysäytetään.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Muutaman minuutin kuluttua se palauttaa suunnilleen tässä esimerkissä:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Jos haluat käynnistää uudelleen virtual tietokoneeseen, jossa on jo käytössä, käytä **Uudelleenkäynnistyksen AzureRmVM** on kuvattu Seuraava.

## <a name="restart-a-virtual-machine"></a>Käynnistä virtual machine

Käynnistä käynnissä virtuaalikoneen.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Se palauttaa suunnilleen tässä esimerkissä:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Poista virtual machine

Poista virtuaalikoneen.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Voit käyttää **-voimassa** parametri ohittaa vahvistusviesti tulee näkyviin.

Jos käytössäsi ei ole voimassa-parametria, sinua pyydetään vahvistamaan:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Se palauttaa suunnilleen tässä esimerkissä:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Päivitä virtual machine

Tässä esimerkissä näytetään päivittämisestä virtuaalikoneen kokoa.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Se palauttaa suunnilleen tässä esimerkissä:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Katso [koot näennäiskoneiden Azure](virtual-machines-windows-sizes.md) virtual machine käytettävissä koot luettelo.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Lisää tietoja DVD-levyllä virtual machine

Tässä esimerkissä näytetään tietojen DVD-levyllä lisääminen aiemmin luotuun virtuaalikoneen.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Levy, jotka olet lisännyt ei ole valmisteltu. Alusta levy, voit kirjautua sisään siihen ja levyn hallinnalla. Jos olet asentanut WinRM ja sertifikaatin sen luomisen yhteydessä, voit PowerShell-Etäistunto alusta levy. Voit myös käyttää mukautettua komentosarjaa tunniste on: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Komentosarjatiedosto voi olla esimerkiksi alustaa levyjä koodi:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Jos käyttöönoton ongelmat, voi tarkastella [vianmääritys resurssin ryhmän ominaisuuksissa Azure-portaalissa](../resource-manager-troubleshoot-deployments-portal.md)
