<properties 
   pageTitle="Yleisiä PowerShell-komennot VMs | Microsoft Azure"
   description="Pääset alkuun luontiin ja hallintaan Windows Azure oman VMs yleisiä PowerShell-komennot"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Yleiset luomisen ja ylläpidon VMs PowerShell-komennot

Tässä artikkelissa käsitellään joitakin PowerShellin Azure-komentoja, joiden avulla voit luoda ja hallita näennäiskoneiden Azure-tilaukseesi.  Tarkempia ohjeita komentorivin valitsimet ja asetukset voit käyttää **Ohjeita** *komento*.

Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja PowerShellin Azure uusimman version asentamisesta, valitsemalla tilauksen ja tiliisi kirjautuminen.

## <a name="create-a-vm"></a>Luo AM

Tehtävä | Komento
-------------- | -------------------------
Luo AM asetukset | $vm = [Uusi AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "vm_name" - VMSize "vm_size"<BR></BR><BR></BR>Voit määrittää tai päivittää AM asetuksia käytetään AM-määritys. Kokoonpano on alustaa nimi AM ja sen [kokoa](virtual-machines-windows-sizes.md).
Lisää asetuksia | $vm = [Määrittäminen AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - AM $vm-Windows - tietokoneen nimi "tietokoneen_nimi"-tunnistetietojen $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Käyttöjärjestelmän asetukset, mukaan lukien [tunnistetiedot](https://technet.microsoft.com/library/hh849815.aspx) lisätään kokoonpano-objekti, jonka loit aiemmin käyttämällä uutta AzureRmVMConfig.
Lisää verkko-käyttöliittymä | $vm = [Lisää AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - AM $vm-tunnuksen $ NIC-sivustosta. Tunnus<BR></BR><BR></BR>AM on oltava [verkkoliittymän](virtual-machines-windows-ps-create.md) vaihtamaan virtual verkossa. Voit myös hakea aiemmin luodun verkon käyttöliittymän objektin [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) .
Määritä platform-kuva | $vm = [Määrittäminen AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - AM $vm - PublisherName "publisher_name"-tarjota "publisher_offer" - tuotteissa "product_sku"-"viimeisimmät-versio<BR></BR><BR></BR>[Kuva tiedot](virtual-machines-windows-cli-ps-findimage.md) lisätään kokoonpano-objekti, jonka loit aiemmin käyttämällä uutta AzureRmVMConfig. Tämä komento palauttaa objektin käytetään vain, kun määrität OS levyä käyttämään ympäristö kuva.
Määritä OS levyä käyttämään platform-kuva | $vm = [Määrittäminen AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - AM $vm-http://mystore1.blob.core.windows.net/vhds/disk_name.vhd"nimi"disk_name"- VhdUri" - CreateOption FromImage<BR></BR><BR></BR>Käyttöjärjestelmä-levyn ja [tallennustilaa](../storage/storage-powershell-guide-full.md) sijainti nimi lisätään aiemmin luomasi kokoonpano-objekti.
Määritä OS levyä käyttämään generalized kuva | $vm = määrittäminen AzureRmVMOSDisk - AM $vm-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk"nimi"disk_name"- SourceImageUri. {guid} .vhd"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>Käyttöjärjestelmän levy, lähteen kuvan sijainti ja kalenteritiedoston sijaintiin [tallennustilaan](../storage/storage-powershell-guide-full.md) nimi lisätään kokoonpano-objekti.
Määritä OS levyä käyttämään tietynlaista kuva | $vm = määrittäminen AzureRmVMOSDisk - AM $vm-http://mystore1.blob.core.windows.net/vhds/"nimi"name_of_disk"- VhdUri" - CreateOption Liitä - Windows
Luo AM | [Uusi AzureRmVM]() - ResourceGroupName "resource_group_name"-"location_name" sijainti - AM $vm<BR></BR><BR></BR>Kaikki resurssit luodaan [resurssiryhmä](../powershell-azure-resource-manager.md). AM on luotava resurssiryhmän samaan [sijaintiin](https://msdn.microsoft.com/library/azure/dn495177.aspx) . Ennen kuin suoritat tämän komennon, suorita uusi AzureRmVMConfig, Määritä AzureRmVMOperatingSystem, Määritä AzureRmVMSourceImage, Lisää AzureRmVMNetworkInterface ja määrittäminen AzureRmVMOSDisk.

## <a name="get-information-about-vms"></a>VMs koskevien tietojen hakeminen

Tehtävä | Komento
-------------- | -------------------------
Luettelon VMs-tilaus| [Hae AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Resurssiryhmä-luettelosta VMs | Hae AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Jos haluat resurssiryhmien luettelo-tilaukseesi, käytä [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
Tietojen AM tarkasteleminen | Hae AzureRmVM - ResourceGroupName "resource_group_name"-nimen "vm_name"

## <a name="manage-vms"></a>VMs hallinta

Tehtävä | Komento
-------------- | -------------------------
Käynnistä AM | [Käynnistä AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-nimen "vm_name"
Lopeta AM | [Lopeta AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-nimen "vm_name"
Käynnistä käynnissä AM | [Käynnistä AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-nimen "vm_name"
Poista AM | [Poista AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-nimen "vm_name"
Generalize AM | [Määritä AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-nimen "vm_name"-Generalized<BR></BR><BR></BR>Suorita tämä komento, ennen kuin suoritat Tallenna AzureRmVMImage.
Sieppaa AM | [Tallenna AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-polku "C:\filepath\filename.json"<BR></BR><BR></BR>Virtual tietokoneen on oltava [Sammuta ja generalized](virtual-machines-windows-generalize-vhd.md) voidaan luoda kuva. Ennen kuin suoritat tämän komennon, suorita määrittäminen AzureRmVm.
Päivitä AM | [Päivitä AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - AM $vm<BR></BR><BR></BR>Hae käyttämällä Hae AzureRmVM AM määrittämisen muuttaminen AM objektin asetukset ja suorita tämä komento.
Lisää tietoja DVD-levyllä AM | [Lisää AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - AM $vm-https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"nimi"disk_name"- VhdUri" - LUN #-välimuistin ReadWrite - DiskSizeinGB # - CreateOption tyhjä<BR></BR><BR></BR>Hae AzureRmVM avulla voit hakea AM objektin. Määritä LUN-numero ja levyn kokoa. Suorita päivitys-AzureRmVM määritysten muutosten AM. Levy, jotka olet lisännyt ei ole valmisteltu. Katso tietoja valmistellaan levyjä, kun ne lisätään [hallinta Azuren näennäiskoneiden Resurssienhallinta ja PowerShellin avulla](virtual-machines-windows-ps-manage.md).
Poistaa tietoja DVD-levyllä AM | [Poista AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - AM $vm-nimen "disk_name"<BR></BR><BR></BR>Hae AzureRmVM avulla voit hakea AM objektin. Suorita päivitys-AzureRmVM määritysten muutosten AM.
Tunnisteen lisääminen AM | [Määritä AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "resource_group_name"-sijainti "azure_location" - VMName "vm_name"-nimi "extension_name"-Publisher "publisher_name"-"extension_type" tyyppi - TypeHandlerVersion "#. #"-asetukset $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Suorita tämä komento tarvittavat [kokoonpanotietoja](virtual-machines-windows-extensions-configuration-samples.md) tunniste, jonka haluat asentaa.
Poista AM-tunniste | [Poista AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-vm_name"nimi"extension_name"- VMName"

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso perusvaiheet luominen virtual machine [luominen Windows-AM Resurssienhallinta ja PowerShellin avulla](virtual-machines-windows-ps-create.md).

