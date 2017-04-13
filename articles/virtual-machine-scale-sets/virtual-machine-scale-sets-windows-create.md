<properties
    pageTitle="Virtuaalikoneen asteikko joukon luominen PowerShellin avulla | Microsoft Azure"
    description="Virtuaalikoneen asteikko joukon luominen PowerShellin avulla"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Luo määrittäminen käyttämällä PowerShellin Azure Windows virtuaalikoneen asteikko

Seuraavia ohjeita noudattamalla täytön----tyhjät-menetelmän Azure virtuaalikoneen asteikko määrittää siihen luomiseen. Artikkelissa lisätietoja asteikko joukot [Virtuaalikoneen asteikko määrittää yleiskatsaus](virtual-machine-scale-sets-overview.md) .

Olisi kestää noin 30 minuuttia voit tehdä tämän artikkelin ohjeita.

## <a name="step-1-install-azure-powershell"></a>Vaihe 1: Azure PowerShellin asentaminen

Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja PowerShellin Azure uusimman version asentamisesta, valitsemalla tilauksen ja tiliisi kirjautuminen.

## <a name="step-2-create-resources"></a>Vaihe 2: Luo resurssit

Luo uusi asteikko-joukko tarvittavat resurssit.

### <a name="resource-group"></a>Resurssiryhmä

Resurssiryhmä täytyy sisältyä joukon virtuaalikoneen asteikko.

1. Saat luettelon käytettävissä olevat sijainnit, joihin voit sijoittaa resurssien käytettävissä:

        Get-AzureLocation | Sort Name | Select Name

2. Valitse sijainti, joka sopii parhaiten puolestasi, korvaa **$locName** arvo kyseisen sijainnin nimen ja luo sitten muuttujan:

        $locName = "location name from the list, such as Central US"

3. Vaihda **$rgName** arvo, jota haluat käyttää uusi resurssiryhmä ja luo sitten muuttujan nimi: 

        $rgName = "resource group name"
        
4. Luo resurssiryhmän:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Näyttöön tulee suunnilleen tässä esimerkissä:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Tallennustilan tilin

Tallennustilan tilin käytetään virtual machine käyttöjärjestelmän levyn ja diagnostiikan skaalauksen käytettyjen tietojen tallentamiseen. Jos mahdollista, on parhaiden käytäntöjen mukaisesti kannattaa kunkin virtuaalikoneen luotu asteikko joukon tallennustilan-tili. Jollet ole mahdollista, enintään 20 VMs tiliä ja tallennustilaa suunnitteleminen. Tämän artikkelin esimerkissä luodaan kolme näennäiskoneiden kolme tallennustilan-tiliä.

1. Korvaa-tallennustilan tilin **$saName** arvo. Testaa yksilöllisyyden nimi. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Vastaus on **Tosi**, jos ehdotettu nimi on yksilöllinen.

3. Korvaa **$saType** arvo tallennustilan tilin tyyppi ja luo sitten muuttujan:  

        $saType = "storage account type"
        
    Mahdolliset arvot ovat: Standard_LRS, Standard_GRS, Standard_RAGRS tai Premium_LRS.
        
4. Luo tili:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Näyttöön tulee suunnilleen tässä esimerkissä:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Toista vaiheet 1 – 4 kolme tallennustilan tiliä, esimerkiksi myst1, myst2 ja myst3 luomiseen.

### <a name="virtual-network"></a>VPN

Virtuaalinen verkon vaaditaan näennäiskoneiden asteikko-määrittäminen.

1. Vaihda **$subnetName** arvo, jota haluat käyttää virtual verkko-osoitteiden ja luo sitten muuttujan nimi: 

        $subnetName = "subnet name"
        
2. Luo aliverkon määritykset:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Osoitteen etuliitteen saattavat olla erilaiset virtual verkossa.

3. Vaihda **$netName** arvo, jonka haluat virtual verkostossa ja luo sitten muuttujan nimi: 

        $netName = "virtual network name"
        
4. Luo virtuaalisia verkon:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Skaalaa joukon määritys

Sinulla on kaikki resurssit, jotka tarvitset asteikon määrittäminen, luo se nyt.  

1. Vaihda **$ipName** arvo, jota haluat käyttää IP-määritys ja luo sitten muuttujan nimi: 

        $ipName = "IP configuration name"
        
2. IP-määrityksen luominen

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Vaihda **$vmssConfig** arvo, jota haluat käyttää asteikko-asetukset ja luo sitten muuttujan nimi:   

        $vmssConfig = "Scale set configuration name"
        
3. Luo mittakaava joukon määritys:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Tässä esimerkissä näytetään asteikolla kolme näennäiskoneiden on luotu. Artikkelissa [Virtuaalikoneen asteikko määrittää yleiskatsaus](virtual-machine-scale-sets-overview.md) lisätietoja asteikko joukot kapasiteetti. Tämän vaiheen myös näennäiskoneiden (jota kutsutaan SkuName) koon määrittäminen joukon. Jos haluat etsiä koko, joka vastaa tarpeitasi, tarkista [näennäiskoneiden koot](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Lisää verkko-liittymän määritykset asteikko-asetukset:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Näyttöön tulee suunnilleen tässä esimerkissä:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Käyttöjärjestelmä profiili

1. Korvaa tietokoneen nimen etuliitteen, jota haluat käyttää ja luo sitten muuttujan **$computerName** arvo: 

        $computerName = "computer name prefix"
        
2. Korvaa **$adminName** näennäiskoneiden järjestelmänvalvojatilin nimi-arvo ja luo sitten muuttujan:

        $adminName = "administrator account name"
        
3. Korvaa **$adminPassword** arvo tilin salasanan ja luo sitten muuttujan:

        $adminPassword = "password for administrator accounts"
        
4. Käyttöjärjestelmä-profiilin luominen:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Tallennustilan profiili

1. Vaihda **$storageProfile** arvo, jota haluat käyttää tallennustilan profiilin ja luo sitten muuttujan nimi:  

        $storageProfile = "storage profile name"
        
2. Luo muuttujat, jotka määrittävät kuvaa, jos haluat käyttää:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Lisätietoja muiden kuvien käyttäminen, tarkista [Siirry ja valitse Azure virtuaalikoneen kuvat ja Windows PowerShellin Azure-CLI](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Korvaa **$vhdContainers** -arvo levyjä tallennuspaikkaa, kuten "https://mystorage.blob.core.windows.net/vhds" polut sisältävä luettelo ja luo sitten muuttujan:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Tallennustilan-profiilin luominen:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Virtuaalikoneen asteikko määrittäminen

Lopuksi voit luoda asteikko-määrittäminen.

1. Vaihda **$vmssName** arvosta virtuaalikoneen asteikko joukon nimi ja luo sitten muuttujan:

        $vmssName = "scale set name"
        
2. Skaalaa-joukon luominen:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Tässä esimerkissä, jossa näkyy onnistunut käyttöönotto esimerkiksi pitäisi näkyä seuraavat:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Vaihe 3: Tutustu resurssit

Näiden resurssien avulla voit tutustua virtuaalikoneen asteikko joukko, jonka loit:

- Azure-portaali - tietojen rajoitetun on käytettävissä portaalissa.
- [Azure resurssin Explorer](https://resources.azure.com/) - työkalun on paras tutustuminen asteikko määrittäminen nykyisen tilan.
- Azure PowerShell – Käytä tätä komentoa tiedot:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Seuraavat vaiheet

- Hallita, että juuri luomasi tietojen avulla [hallita näennäiskoneiden virtuaalikoneen-asteikko-joukko](virtual-machine-scale-sets-windows-manage.md) asteikko
- Harkitse määrittämistä oman asteikko määrittää tiedot käyttämällä [Automaattinen skaalaus ja virtuaalikoneen asteikko määrittää](virtual-machine-scale-sets-autoscale-overview.md) Automaattinen skaalaus
- Lue lisää [Pystysuuntainen Automaattinen skaalaus virtuaalikoneen asteikko joukot ja](virtual-machine-scale-sets-vertical-scale-reprovision.md) tarkastelemalla skaalauksen pystysuunnassa
