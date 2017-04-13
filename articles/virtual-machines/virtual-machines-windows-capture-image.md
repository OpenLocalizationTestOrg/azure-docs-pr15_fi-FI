<properties
    pageTitle="Generalized Azure AM-kuvan AM | Microsoft Azure"
    description="Lue, miten voit siepata AM kuvan, valitse generalized Azure-AM luotu resurssien hallinnan käyttöönottomalli"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Voit siepata generalized Azure-AM AM kuva


Tämän artikkelin avulla voit luoda generalized Azure-AM kuva Azure PowerShellin avulla. Voit sitten kuvan toiseen AM luomiseen. Kuva sisältää OS levyn ja tietoja-levyjä, jotka on liitetty virtuaalikoneen. Kuvan ei ole virtual verkkoresursseja, joten sinun täytyy määrittää nämä resurssit, kun luot uuden AM. 


## <a name="prerequisites"></a>Edellytykset

- Sinun täytyy vielä ole [generalized AM](virtual-machines-windows-generalize-vhd.md). Joilla AM poistaa kaikki henkilökohtaiset tietosi, muun muassa ja valmistelee tietokone, jota käytetään kuvana.

- Sinun täytyy olla PowerShellin Azure versio 1.0.x tai uudempi versio. Jos et ole jo asentanut PowerShell, lue [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) asennusohjeita.


## <a name="log-in-to-azure-powershell"></a>Azure PowerShell kirjautuminen

1. Avaa PowerShellin Azure ja kirjaudu sisään Azure-tili.

    ```powershell
    Login-AzureRmAccount
    ```

    Ponnahdusikkunan Avaa sinun on annettava Azure tilin tunnistetiedot.

2. Saat käytettävissä tilauksistasi tilauksen tunnukset.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Määritä oikea tilaukseesi tilauksen ID-tunnuksellasi.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Poista varaus AM ja määritä tilaksi generalized       

1. Poista varaus AM resurssit.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    Azure-portaalissa AM *tila* vaihtuu **pysäytetty** **pysäytetty (Varaus poistettu)**.

2. Määritä virtuaalikoneen tilaksi **Generalized**. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Tarkista AM tila. **OSState/generalized** -osassa AM on määritetty **AM generalized** **DisplayStatus** .  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Luo kuva 

1. Kopioi kuva virtuaalikoneen tällä komennolla kohdesäilön tallennustilan. Kuva luodaan saman tallennustilan tilin kuin alkuperäinen virtuaalikoneen. `-Path` Parametrin tallentaa paikallisesti JSON-mallin kopio. `-DestinationContainerName` Parametri on säilö, jonka haluat pidä kuvien nimi. Jos säilö ei ole valittu, se luodaan puolestasi.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    Saat kuvan URL-osoite JSON-tiedostomallin pohjalta. Valitse **resurssit** > **storageProfile** > **osDisk** > **Kuva** > **uri** -osan täydellinen polku kuvan. Kuvan URL-osoite näyttää seuraavalta: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Voit myös tarkistaa URI-portaalissa. Kuva kopioidaan **järjestelmän** tilisi tallennustilan säilö. 


## <a name="next-steps"></a>Seuraavat vaiheet

- Nyt voit [luoda AM kuvasta](virtual-machines-windows-create-vm-generalized.md).

