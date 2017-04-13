<properties
   pageTitle="Ottaa käyttöön usean NIC VMs resurssien hallinta PowerShellin avulla | Microsoft Azure"
   description="Lue, miten voit ottaa usean NIC VMs resurssien hallinta PowerShellin avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-powershell"></a>Ottaa käyttöön usean NIC VMs PowerShellin avulla

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönottomalli](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Ei voi tällä hetkellä on VMs yksittäisen NIC kanssa ja VMs, jossa on useita NIC käytettävyys samat. Tämän vuoksi haluat toteuttavien uudelleen palvelimissa eri kuin muiden osien resurssiryhmä. Seuraavia ohjeita käyttää nimetyt *IaaSStory* tärkeimmät resurssiryhmä ja *IaaSStory Taustajärjestelmä* uudelleen palvelimia resurssiryhmä.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit ottaa käyttöön uudelleen palvelimissa, sinun täytyy käyttöönotto ja kaikki tarvittavat resurssit tämän skenaarion tärkeimmät resurssiryhmä. Ottaa käyttöön näiden resurssien, noudata seuraavia ohjeita.

1. Siirry [mallisivun](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Valitse mallisivun oikealta puolelta **ylätason resurssiryhmä** **käyttöönotto Azure**.
3. Tarvittaessa Muuta kaavarivillä parametriarvot ja noudata ohjeita ottamaan resurssiryhmän Azure esikatselu-portaalissa.

> [AZURE.IMPORTANT] Varmista, että tallennustilan tilin nimet ovat yksilöllisiä. Kaksoiskappaleiden tallennustilan tilin nimiä ei voi olla Azure-tietokannassa.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Edellinen loppuun VMs käyttöönotto

Taustajärjestelmä VMs määräytyvät alla luetellut resurssit luomista.

- **Tallennustilan tilin tiedot levyille**. Parantaa suorituskykyä tietoja levyjen tietokannan palvelimissa, jotka käyttävät tasainen tilan asemaa Suoritettaessa tekniikka, jossa pyydetään premium-tallennustilan tilin. Varmista, että käyttöönottoa premium tallennustilan tukemaan Azure sijainti.
- **NIC**. Kunkin AM on kaksi NIC-tietokannan käyttämiseen ja yksi hallintaa varten.
- **Saatavuus**. Tietokantapalvelin lisätään yksi käytettävyys määrittäminen, jotta vähintään yksi VMs on ylös ja suorittamalla ylläpidon aikana.  

### <a name="step-1---start-your-script"></a>Vaihe 1 – komentosarja

Voit ladata käytetään koko PowerShell-komentosarjaa [tähän](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1). Noudata seuraavia ohjeita voit muuttaa toimimaan ympäristössäsi komentosarja.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Muuttaa arvoja muuttujien, otettu käyttöön edellä [edellytykset](#Prerequisites)aiemmin resurssiryhmän perusteella.

        $existingRGName        = "IaaSStory"
        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"
        $remoteAccessNSGName   = "NSG-RemoteAccess"
        $stdStorageAccountName = "wtestvnetstoragestd"

2. Muuttaa arvoja muuttujien, jota haluat käyttää Taustajärjestelmä käyttöönottoa varten arvojen perusteella.

        $backendRGName         = "IaaSStory-Backend"
        $prmStorageAccountName = "wtestvnetstorageprm"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $publisher             = "MicrosoftSQLServer"
        $offer                 = "SQL2014SP1-WS2012R2"
        $sku                   = "Standard"
        $version               = "latest"
        $vmNamePrefix          = "DB"
        $osDiskPrefix          = "osdiskdb"
        $dataDiskPrefix        = "datadisk"
        $diskSize              = "120"  
        $nicNamePrefix         = "NICDB"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

3. Noutaa tarvittavat käyttöönoton olemassa olevat resurssit.

        $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
        $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
        $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
        $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Vaihe 2: Luo tarvittavat resurssit oman VMs

Sinun on luotava uusi resurssiryhmä-tallennustilan tilin tiedot-levyille ja kaikki VMs määrittäminen saatavuus. Alos tarvitset paikallisen järjestelmänvalvojan tilin tunnistetiedot kunkin AM. Luo nämä resurssit, suorita seuraavat vaiheet.

1. Luo uusi resurssiryhmä.

        New-AzureRmResourceGroup -Name $backendRGName -Location $location

2. Luo uuden premium-tallennustilan tilin edellä luotu resurssiryhmä.

        $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
            -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location

3. Luo uusi joukko käytettävyyttä.

        $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location

4. Pyydä paikallisen järjestelmänvalvojan tilin tunnistetiedot, jota käytetään kunkin AM.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

### <a name="step-3---create-the-nics-and-backend-vms"></a>Vaihe 3 – NIC- ja Taustajärjestelmä VMs luominen

Haluat muodostaa silmukan avulla voit luoda niin monta VMs ja luo tarvittavat NIC- ja VMs silmukassa. Luo NIC- ja VMs, suorita seuraavat vaiheet.

1. Käynnistä-painiketta `for` silmukka toista komentoja AM ja kaksi NIC niin monta kertaa tarvittaessa arvon perusteella `$numberOfVMs` muuttuja.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Voit luoda tietokantoja käytettäviä NIC.

            $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
            $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
            $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1

3. Luo käytetään etäkäyttöä varten NIC. Huomaa, miten tämä NIC on siihen liittyvä NSG.

            $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
            $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
            $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
                -NetworkSecurityGroupId $remoteAccessNSG.Id

4. Luo `vmConfig` objekti.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id

5. Luo kaksi tietojen levyä AM kohden. Huomaa, että tietojen levyjen on aiemmin luotu premium-tallennustilan tilin.

            $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"    
            $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
                -VhdUri $data1VhdUri -CreateOption empty -Lun 0

            $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"    
            $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
                -VhdUri $data2VhdUri -CreateOption empty -Lun 1

6. Määritä käyttöjärjestelmä ja kuva, jota käytetään AM.

            $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
            $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version

7. Lisää kaksi NIC luotu edellä `vmConfig` objekti.

            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id

8. Luo OS levyn ja luo AM. Ilmoitus `}` , jonka pääte `for` silmukka.

            $osDiskName = $vmName + "-" + $osDiskSuffix
            $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
            $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
            New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
        }

### <a name="step-4---run-the-script"></a>Vaihe 4 - komentosarjan suorittaminen

Ladattu ja muuttaa käyttötarkoitukseen komentosarja, runt hän komentosarja useita NIC tietokannan VMs luoda uudelleen.

1. Tallenna komentosarja ja suorita **PowerShell** komentokehote tai **PowerShell ise:**. Alkuperäinen tulos näkyy alla kuvatulla tavalla.

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        ResourceId        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/IaaSStory-Backend

2. Muutaman minuutin kuluttua täytä tunnistetietojen kehote ja valitse **OK**. Alla olevassa tulosteen edustaa yhden AM. Ilmoitus koko prosessin kulunut 8 minuuttia.

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        EndTime             : 10/30/2015 9:30:03 AM -08:00
        Error               :
        Output              :
        StartTime           : 10/30/2015 9:22:54 AM -08:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
