<properties
    pageTitle="Koon muuttaminen Windowsin AM | Microsoft Azure"
    description="Muuta Windows virtual machine-resurssien hallinnan käyttöönotto mallissa Azure PowerShellin avulla luotu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Ikkunoiden AM koon muuttaminen

Tässä artikkelissa kerrotaan Windows AM Resurssienhallinta käyttöönoton mallin Azure PowerShellin avulla luotu koon muuttaminen.

Kun olet luonut virtual machine (AM), voit skaalata AM ylös tai alas muuttamalla AM. Joissakin tapauksissa sinun täytyy poistaa varauksen AM ensin. Näin voi käydä, jos uusi koko ei ole käytettävissä, joka isännöi tällä hetkellä AM laitteisto-klusterin.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Windows-AM kuin käytettävyys määrittäminen koon muuttaminen

1. Luettele AM koot, jotka ovat käytettävissä mihin AM nykyisessä laitteisto-klusterin. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Jos luettelossa on halutun kokoinen, suorita seuraavat komennot koon AM. Jos haluamasi kokoiseksi ei näy luettelossa, siirry vaiheeseen 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Jos haluamasi kokoiseksi ei näy luettelossa, suorita seuraavat komennot poistaa varauksen AM, muuta sen kokoa ja alusta AM.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Dynaaminen IP-osoitteita AM myönnetyt vapauttamista AM versiot. OS ja tietoja-levyjen näin ei käy. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Windows-AM käytettävyys määrittäminen koon muuttaminen

Jos uusi koon AM käytettävyys määrittäminen ei ole käytettävissä isännöinnin tällä hetkellä AM laitteiston klusterin, kaikki VMs käytettävyys määrittäminen on varaus on poistettu AM kokoa. Voit myös joutua päivittämään muiden VMs koon määrittäminen, kun yksi AM on muutettu käytettävyys. Koon muuttaminen AM käytettävyys määrittäminen toimimalla seuraavasti.

1. Luettele AM koot, jotka ovat käytettävissä mihin AM nykyisessä laitteisto-klusterin.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Jos luettelossa on halutun kokoinen, suorita seuraavat komennot koon AM. Jos se ei näy luettelossa, siirry vaiheeseen 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Jos haluamasi kokoiseksi ei näy luettelossa, jatka seuraavien ohjeiden avulla poistaa kaikki VMs käytettävyys määrittäminen varauksen, VMs kokoa ja käynnistä ne uudelleen.

4.  Lopeta kaikki VMs käytettävyys määrittäminen.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Kokoa ja Käynnistä VMs käytettävyys määrittäminen.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisää skaalattavuus suorittaa useita AM esiintymät ja skaalata ulos. Lisätietoja on artikkelissa [Skaalaa automaattisesti virtuaalikoneen-asteikko-joukko Windows-tietokoneissa](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).



