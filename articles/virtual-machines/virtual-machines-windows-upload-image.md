<properties
    pageTitle="Lataa Windows Näennäiskiintolevyn resurssien hallinnan | Microsoft Azure"
    description="Lisätietoja Windows virtual machine Näennäiskiintolevyn-paikallisen Azure-resurssien hallinnan käyttöönotto-mallin lataaminen. Voit ladata-joko generalized Näennäiskiintolevyn tai erityinen AM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Windows Azure, Näennäiskiintolevyn paikallisen AM-Lataa 


Tässä artikkelissa kerrotaan, miten voit luoda ja ladata Windows virtual kiintolevyn (Näennäiskiintolevyn) käytettäviksi luominen Azure-AM. Voit ladata Näennäiskiintolevyn generalized AM tai erityinen AM. 

Lisätietoja levyjen ja Azure näennäiskiintolevyjen artikkelissa [levyille ja näennäiskiintolevyjen näennäiskoneiden](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>Valmistele AM 

Voit ladata generalized sekä erityisiä näennäiskiintolevyjen Azure. Kunkin edellyttää valmisteleminen AM ennen aloittamista.

- **Generalized Näennäiskiintolevyn** - generalized Näennäiskiintolevyn on ollut kaikki henkilökohtaiset tietosi poistetaan Sysprepin avulla. Jos haluat luoda uuden VMs-kuvana Näennäiskiintolevyn avulla, sinun on:
    - [Valmistele Windows Azure lataaminen Näennäiskiintolevyn](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Yleistä virtuaalikoneen Sysprepin avulla](virtual-machines-windows-generalize-vhd.md). 

- **Tietynlaista Näennäiskiintolevyn** - tietynlaista Näennäiskiintolevyn ylläpitää käyttäjätilit, sovellusten ja alkuperäinen AM tilan muut tiedot. Jos aiot käyttää Näennäiskiintolevyn nimellä-voidaan luoda uusi AM, varmista seuraavat vaiheet on suoritettu. 
    - [Valmistele Windows Azure lataaminen Näennäiskiintolevyn](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Älä** generalize AM Sysprepin avulla.
    - Poista kaikki Vieras virtualization työkalut ja agenttien vuoksi, jotka on asennettu AM (eli VMware työkalut).
    - Varmista, AM on määritetty hakemaan sen IP-osoite ja DNS-asetukset DHCP kautta. Näin varmistat, että palvelin hakee VNet IP-osoitteen käynnistyksen yhteydessä. 

## <a name="log-in-to-azure"></a>Azure kirjautuminen

Jos sinulla ei vielä ole PowerShell versio 1.4 tai asennettuna, lue [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)yläpuolella.

1. Avaa PowerShellin Azure ja kirjaudu sisään Azure-tili. Ponnahdusikkunan Avaa sinun on annettava Azure tilin tunnistetiedot.

    ```powershell
    Login-AzureRmAccount
    ```


2. Saat käytettävissä tilauksistasi tilauksen tunnukset.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Määritä oikea tilaukseesi tilauksen ID-tunnuksellasi. Korvaa `<subscriptionID>` oikea tilauksen tunnus.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Tallennustilan-tilin hankkiminen

Tarvitset ladatut AM kuvan tallentamiseen Azure-tallennustilan tilin. Voit käyttää käytössä olevan tallennustilan-tilin tai luoda uuden. 

Jos haluat näyttää käytettävissä olevan tallennustilan tilit, kirjoita:

```powershell
Get-AzureRmStorageAccount
```

Jos haluat käyttää käytössä olevan tallennustilan tilin, siirry [AM kuvan lataaminen](#upload-the-vm-vhd-to-your-storage-account) -kohtaan.

Jos haluat luoda tallennustilan tilin, toimi seuraavasti:

1. Tarvitset resurssiryhmä, jossa luotu tallennustilan tilin nimi. Saat selville kaikkia resurssiryhmiä, jotka ovat tilauksen, kirjoita:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Voit luoda resurssiryhmän nimeltä **myResourceGroup** **Länsi US** alueen tyyppi:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Luo tallennustilan tili nimeltä **mystorageaccount** resurssi-ryhmässä [Uusi AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) cmdlet-komennolla:

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Kelvolliset - SkuName arvot ovat:

    - **Standard_LRS** - paikallisesti ylimääräinen. 
    - **Standard_ZRS** - vyöhykkeen tarpeettomat tallennustilan.
    - **Standard_GRS** - Geo tarpeettomat tallennustilan. 
    - **Standard_RAGRS** - lukuoikeudet geo tarpeettomat-tallennustilan. 
    - **Premium_LRS** - Premium paikallisesti tarpeettomat tallennustilan. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Lataa Näennäiskiintolevyn tallennustilan tilin

Kuvan lataaminen tallennustilan tilisi säilön [Lisää AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) -cmdlet-komennolla. Tässä esimerkissä latauksia-tiedoston **myVHD.vhd** `"C:\Users\Public\Documents\Virtual hard disks\"` säilöön tili nimeltä **mystorageaccount** **myResourceGroup** resurssiryhmän. Tiedoston sijoitetaan nimeltä **mycontainer** säilöön ja uusi tiedostonimi päivitetään **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Jos onnistuu, näyttöön tulee vastauksen, joka näyttää seuraavankaltaiselta:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Verkkoyhteys ja .vhd-tiedoston koon mukaan tämä komento voi kestää jonkin aikaa suorittamiseen


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure-generalized Näennäiskiintolevyn AM luominen](virtual-machines-windows-create-vm-generalized.md)
- [Luo AM-Azure-erityinen Näennäiskiintolevyn](virtual-machines-windows-create-vm-specialized.md) liität kuin OS-levyn, kun luot uuden AM.


