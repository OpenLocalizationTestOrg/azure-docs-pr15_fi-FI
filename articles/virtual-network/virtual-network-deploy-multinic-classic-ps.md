<properties
   pageTitle="Usean NIC VMs PowerShellin perinteinen käyttöönoton mallin käyttöön | Microsoft Azure"
   description="Lue, miten voit ottaa usean NIC VMs perinteinen käyttöönoton mallin PowerShellin avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Ottaa käyttöön usean NIC VMs (perinteinen) PowerShellin avulla

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Voit luoda näennäiskoneiden (VMs) Azure ja liitä kunkin käyttäjän VMs useita verkkoliittymät (NIC). Useita NIC käyttöön tietoliikenteen eri erottaminen NIC yli. Esimerkiksi yksi NIC ehkä yhteydessä Internetiin, kun toiseen kommunikoi osallistuvan sisäiset resurssit ei ole yhteydessä Internetiin. Mahdollisuus verkkoliikenteestä erota yli useita NIC vaaditaan monta verkon virtual laitteita, kuten sovelluksen toimitus- ja WAN optimointimahdollisuudet.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Ei voi tällä hetkellä olla VMs yksittäisen NIC kanssa ja VMs, jossa on useita NIC saman pilvipalvelussa. Tämän vuoksi haluat toteuttavien uudelleen palvelimissa eri pilvipalvelussa kuin ja muiden osien skenaariossa. Seuraavia ohjeita käyttäminen-nimiseen *IaaSStory* tärkeimmät resursseja ja *IaaSStory Taustajärjestelmä* uudelleen palvelimia pilvipalveluun.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit ottaa käyttöön uudelleen palvelimissa, sinun täytyy ottaa käyttöön sisällön pilvipalvelussa sijaitsevaan ja kaikki tarvittavat resurssit tässä tapauksessa. Vähintään, sinun täytyy luoda virtuaalisen verkon taustaan aliverkon. Käy virtual verkon opetteleminen [Luo virtuaalisia verkon PowerShell-toiminnolla](virtual-networks-create-vnet-classic-netcfg-ps.md) .

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Edellinen loppuun VMs käyttöönotto

Taustajärjestelmä VMs määräytyvät alla luetellut resurssit luomista.

- **Taustajärjestelmä aliverkon**. Tietokannan palvelimet on erillinen aliverkon, voit eroteltava liikenne osa. Alla olevaa komentosarjaa odottaa tämän aliverkon vnet, jonka nimi on *WTestVnet*olemassa.
- **Tallennustilan tilin tiedot levyille**. Parantaa suorituskykyä tietoja levyjen tietokannan palvelimissa, jotka käyttävät tasainen tilan asemaa Suoritettaessa tekniikka, jossa pyydetään premium-tallennustilan tilin. Varmista, että käyttöönottoa premium tallennustilan tukemaan Azure sijainti.
- **Saatavuus**. Tietokantapalvelin lisätään yksi käytettävyys määrittäminen, jotta vähintään yksi VMs on ylös ja suorittamalla ylläpidon aikana.

### <a name="step-1---start-your-script"></a>Vaihe 1 – komentosarja

Voit ladata käytetään koko PowerShell-komentosarjaa [tähän](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Noudata seuraavia ohjeita voit muuttaa toimimaan ympäristössäsi komentosarja.

1. Muuttaa arvoja muuttujien, otettu käyttöön edellä [edellytykset](#Prerequisites)aiemmin resurssiryhmän perusteella.

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Muuttaa arvoja muuttujien, jota haluat käyttää Taustajärjestelmä käyttöönottoa varten arvojen perusteella.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Vaihe 2: Luo tarvittavat resurssit oman VMs

Haluat luoda uuden pilvipalvelussa ja tallennustilaa tilin tiedot-levyjen kaikki VMs. Haluat myös kuvan ja paikallisen järjestelmänvalvojatilin määrittäminen VMs. Luo nämä resurssit, suorita seuraavat vaiheet.

1. Luo uusi pilvipalvelussa.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Luo uusi premium tallennustilan tili.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Määritä luotu edellä nykyistä tallennustilan tilinä tilauksen tallennustilan tili.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Valitse kuva AM.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Määrittää paikallisen järjestelmänvalvojan tilin tunnistetiedot.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Vaihe 3 – VMs luominen

Haluat muodostaa silmukan avulla voit luoda niin monta VMs ja luo tarvittavat NIC- ja VMs silmukassa. Luo NIC- ja VMs, suorita seuraavat vaiheet.

1. Käynnistä-painiketta `for` silmukka toista komentoja AM ja kaksi NIC niin monta kertaa tarvittaessa arvon perusteella `$numberOfVMs` muuttuja.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Luo `VMConfig` objektin määrittäminen kuva, koko ja käytettävyyden asettaminen AM.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. Valmistele AM Windows AM nimellä.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. Asettaminen NIC ja määritä se staattinen IP-osoite.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Lisää toinen NIC kunkin AM.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Luo tietojen levyille kunkin AM.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Kunkin AM aloittamisesta ja lopettamisesta tapahtuu.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Vaihe 4 - komentosarjan suorittaminen

Ladattu ja muuttaa käyttötarkoitukseen komentosarja, runt hän komentosarja useita NIC tietokannan VMs luoda uudelleen.

1. Tallenna komentosarja ja suorita **PowerShell** komentokehote tai **PowerShell ise:**. Alkuperäinen tulos näkyy alla kuvatulla tavalla.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Täytä tunnistetietojen kehote tarvittavat tiedot ja valitse **OK**. Alla olevassa tulos tulee näkyviin.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
