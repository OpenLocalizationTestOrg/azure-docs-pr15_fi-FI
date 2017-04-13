<properties
   pageTitle="Voit hallita oman näennäiskoneiden PowerShellin Azure | Microsoft Azure"
   description="Lue komentoja, joiden avulla voit automatisoida tehtäviä oman näennäiskoneiden hallinta."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Oman näennäiskoneiden hallinta PowerShellin Azure avulla

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Azure PowerShell cmdlet-komentojen avulla voidaan automatisoida useita tehtäviä voit hallita oman VMs päivittäin. Tässä artikkelissa tutustutaan Esimerkki komennoista yksinkertaisempi tehtäviä ja linkit artikkeleihin, joissa on monimutkaisia tehtävien komentojen näyttäminen.

>[AZURE.NOTE] Jollet ole vielä asentanut ja määrittänyt PowerShellin Azure, saat ohjeet on artikkelissa [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

## <a name="how-to-use-the-example-commands"></a>Esimerkki-komentojen käyttäminen
Sinun on joitakin komentoja tekstin korvaava teksti, joka sopii ympäristössä. < Ja > Määritä haluat korvata tekstiä, merkit. Kun korvaat tekstiä, poistaa symbolit, mutta jättää paikoilleen, tarjouksen olevat lainausmerkit.

## <a name="get-a-vm"></a>Hae AM
Tämä on basic tehtävän käytät usein. Käytä tietojen tarkasteleminen AM, suorittaa tehtäviä AM tai Hae tulosteen tallentamiseen muuttujana.

Saat lisätietoja AM Suorita tämä komento korvaa kaikki tarjoukset, mukaan lukien < ja > merkit:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Voit tallentaa tulosteen $vm muuttuja, suorittamalla:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Kirjaudu Windows-pohjaisesta AM

Suorita nämä komennot:

>[AZURE.NOTE] Saat virtuaalikoneen ja pilvi palvelunimi **Hae AzureVM** -komennossa.
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Lopeta AM

Suorita tämä komento:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Tämä parametri avulla voit säilyttää pilvipalvelussa virtual IP (VIP) siltä varalta, että pilvipalvelussa viimeisen AM. <br><br> Jos käytät StayProvisioned-parametri, sinun silti laskutetaan AM.

## <a name="start-a-vm"></a>Käynnistä AM

Suorita tämä komento:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Liitä tiedot DVD-levyllä
Tämä tehtävä edellyttää muutama vaihe. Ensimmäisen kerran, voit käyttää *** Lisää-AzureDataDisk *** cmdlet-komento levyn lisääminen $vm objekti. Tämän jälkeen **Päivityksen AzureVM** cmdlet-komento käyttää päivittämään AM määritys.

Sinun on myös valitseminen liittää uuden levyn tai joka sisältää tiedot. Uuden levyn komento luo .vhd-tiedoston, ja liittää sen.

Jos haluat liittää uuden levyn, suorita tämä komento:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Jos haluat liittää tiedot olemassa olevaa levyä, suorita tämä komento:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Jos haluat liittää tiedot levyjen aiemmin luodusta .vhd Blob-objektien tallennustilaan, suorita tämä komento:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Windows-pohjaisesta AM luominen

Voit luoda uuden Windows-pohjaisesta virtual koneen Azure-tietokannassa käyttämällä [Azure](virtual-machines-windows-classic-create-powershell.md)PowerShellin avulla voit luoda ja määrittää valmiiksi Windows-pohjaisesta näennäiskoneiden ohjeita. Tämän ohjeaiheen vaiheet läpi PowerShellin Azure komento, joka määrittää luominen luo Windows-pohjaisesta AM, joka määrittää valmiiksi:

- Kanssa Active Directory-toimialueen jäsenyyden.
- Kanssa muita levyjä.
- Aiemmin luodun kuormituksen jäsenenä määrittäminen.
- Staattinen IP-osoite.
