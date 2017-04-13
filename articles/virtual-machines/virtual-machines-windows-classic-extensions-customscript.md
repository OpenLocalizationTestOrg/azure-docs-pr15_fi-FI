<properties
   pageTitle="Mukautettu komentosarja tunniste Windows AM | Microsoft Azure"
   description="Mukautettu komentosarja-tunniste avulla voidaan suorittaa PowerShell-komentosarjojen remote Windows-AM Azure AM määritysten tehtävien automatisointi"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Windowsin näennäiskoneiden mukautettu komentosarja-laajennus

Tässä artikkelissa on yleiskatsaus mukautettu komentosarja-tunniste käyttämisestä Windows VMs Azure Service Management API Azure PowerShell cmdlet-komentoja käyttämällä.

Virtual machine (AM)-laajennukset on Microsoftin ja luotetut kolmannen osapuolen julkaisijat AM toimintoja voidaan laajentaa. Katso yleiskuvaus AM tunnisteet- [Azure AM tunnisteet ja toiminnot](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lisätietoja [näiden ohjeiden avulla Resurssienhallinta-mallin](virtual-machines-windows-extensions-customscript.md)tietoja.

## <a name="custom-script-extension-overview"></a>Mukautettu komentosarja-tunniste yleiskatsaus

Windowsin mukautettu komentosarja-tunniste voit suorittaa PowerShell-komentosarjojen remote AM kirjautumatta sisään siihen. Voit suorittaa komentosarjat AM valmistelun jälkeen tai milloin tahansa AM elinkaaren aikana kaikki muut portit avaamatta. Yleisimmät Käytä palvelupyynnöt käynnissä mukautettu komentosarja-tunniste ovat käynnissä, asentaminen ja määrittäminen lisäohjelmistoa AM, kun se on valmisteltu.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Mukautettu komentosarja-tunniste suorittamisen edellytykset

1. Asenna <a href="http://azure.microsoft.com/downloads" target="_blank">Azure PowerShellin cmdlet-komennot</a> 0.8.0 versio tai sitä uudemmalla versiolla.
2. Jos haluat suorittaa aiemmin AM komentosarjat, varmista, että AM agentti on otettu käyttöön AM. Jos sitä ei ole asennettu, noudattamalla seuraavia [ohjeita](virtual-machines-windows-classic-agents-and-extensions.md). Jos AM luodaan Azure-portaalista, AM agentti asennetaan oletusarvoisesti.
3. Lataa komentosarjat, jonka haluat suorittaa Azure säilöön AM. Komentosarjat voi tulla yksi säilön tai useita tallennustilan säiliöiden.
4. Komentosarja on tehty, niin, että tapahtuma-komentosarjan, joka on käynnistetty tunnisteen mukaan, käynnistää muita komentosarjoja.

## <a name="custom-script-extension-scenarios"></a>Mukautettu komentosarja tunniste skenaariot

### <a name="upload-files-to-the-default-container"></a>Tiedostojen lataaminen oletusarvo-säilö

Seuraavassa esimerkissä esitetään, miten voit suorittaa komentosarjat AM jos ne eivät oletustilin tilauksen tallennustilan säilö. Voit ladata komentosarjoja ContainerName. Voit tarkistaa tallennustilan oletustilin avulla **Get-AzureSubscription – oletusarvon** komento.

Seuraavassa esimerkissä luodaan AM, mutta voit myös suorittaa sama skenaario aiemmin AM.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Tiedostojen lataaminen muun kuin oletusarvoisen tallennustilan säilö

Tässä skenaariossa esitetään, kuinka voit käyttää muun kuin oletusarvoisen tallennustilan säilö saman tilauksen piiriin kuuluvien tai eri tilauksen komentosarjojen ja tiedostojen lataaminen palvelimeen. Tässä esimerkissä näytetään aiemmin AM, mutta samat toiminnot voidaan toteuttaa, kun olet luomassa AM.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Lataa useita säilöjen komentosarjojen tallennustilan eri tilien välillä

  Jos komentosarjatiedostot on tallennettu useita säilöjen välillä, on annettava jaetun täysien allekirjoitukset (SAS) URL-osoite tiedostot komentosarjojen suorittamisen.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Lisää mukautettu komentosarja-tunniste Azure-portaalista

Siirry <a href="https://portal.azure.com/ " target="_blank">Azure portal</a> AM ja Lisää tunniste määrittämällä komentosarjatiedosto suorittamiseen.

  ![Määritä komentosarjatiedosto][5]


### <a name="uninstall-the-custom-script-extension"></a>Poista mukautettu komentosarja-tunniste

Mukautettu komentosarja-tunniste voit poistaa AM käyttämällä seuraavaa komentoa.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Käytä malleja mukautettu komentosarja-tunniste

Lisätietoja käytettävä mukautettu komentosarja-tunniste Azure Resurssienhallinta-mallien käyttämisestä on ohjeaiheessa [Windows Azure Resurssienhallinta malleja VMs mukautettu komentosarja-laajennus](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
