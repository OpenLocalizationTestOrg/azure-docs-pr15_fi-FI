<properties
    pageTitle="Luo PowerShellin Azure-AM | Microsoft Azure"
    description="PowerShellin Azure ja Azure resurssien hallinnan avulla voit helposti luoda uuden AM, joissa on käytössä Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Luo Windows-AM Resurssienhallinta ja PowerShellin avulla

Tässä artikkelissa kerrotaan, miten voit luoda nopeasti Azure Virtual Machine on käynnissä Windows Server ja resurssit, se tarvitsee [Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) ja PowerShellin avulla. 

Tässä artikkelissa kuvattuja virtual machine luomiseen tarvitaan ja olisi kestää noin 30 minuuttia toimet. Esimerkki parametriarvot komennoissa tilalle nimet, jotka sopivat-ympäristössä.

## <a name="step-1-install-azure-powershell"></a>Vaihe 1: Azure PowerShellin asentaminen

Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja PowerShellin Azure uusimman version asentamisesta, valitsemalla tilauksen ja tiliisi kirjautuminen.
        
## <a name="step-2-create-a-resource-group"></a>Vaihe 2: Luo resurssiryhmä

Kaikki resurssit täytyy sisältyä resurssiryhmä-avulla niin, että ensin.  

1. Saat luettelon käytettävissä olevat sijainnit-kohtaa, johon voidaan luoda resurssit.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Resurssien sijainnin määrittäminen. Tämä komento määrittää sijainnin **centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Luo resurssiryhmä. Tämä komento luo resurssiryhmän nimeltä **myResourceGroup** määrität haluamaasi kohtaan.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Vaihe 3: Tallennustilan tilin luominen

[Tallennustilan tilin](../storage/storage-introduction.md) tarvitaan virtual kovalevy, jota käytetään virtuaalikoneen, jonka luot tallentamiseen. Tallennustilan tilin nimi on oltava 3 – 24 merkkiä ja voivat sisältää numeroita ja pieniä kirjaimia vain.

1. Testaa yksilöllisyyden tallennustilan tilin nimi. Tämä komento on testannut nimi **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Jos tämä komento palauttaa arvon **Tosi**, ehdotettu nimi on yksilöivä Azure. 
    
2. Nyt-tallennustilan tilin luominen
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Vaihe 4: Virtual verkoston luominen

Kaikki näennäiskoneiden on osa [VPN](../virtual-network/virtual-networks-overview.md).

1. Luo virtuaalisia verkon aliverkon. Tämä komento luo aliverkon nimeltä **mySubnet** 10.0.0.0/24 osoite etuliitteellä.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Seuraavaksi luodaan virtual verkkoon. Tämä komento luo virtuaalisia verkon nimi, jonka loit aliverkon ja osoitteen etuliite, **10.0.0.0/16** **myVnet** .

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Vaihe 5: Julkisen IP-osoite ja verkon liittymän luominen

Virtuaalikoneen virtual verkon tietoliikenteen käyttöön tarvitset [julkiseen IP-osoite](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ja Verkkokäyttöliittymän.

1. Luo julkinen IP-osoite. Tämä komento luo julkinen IP-osoite nimeltä **myPublicIp** kohdistus-menetelmällä ja **dynaaminen**.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Luo Verkkokäyttöliittymän. Tämä komento luo nimeltä verkon-liittymän **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Vaihe 6: Luo virtual machine

Nyt kun olet luonut kaikki osat paikassa, on aika virtuaalikoneen luomiseen.

1. Suorita tällä komennolla voit määrittää järjestelmänvalvojan tilin nimi ja salasana virtuaalikoneen.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Salasana on osoitteessa 12 123 merkkiä ja on vähintään yksi pieni kirjain, yksi iso kirjain, yksi numero ja yksi erikoismerkki. 
        
2. Luo virtuaalikoneen kokoonpano-objekti. Tämä komento luo nimeltä **myVmConfig** , joka määrittää AM nimi ja koon AM kokoonpano-objekti.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Katso [koot näennäiskoneiden Azure](virtual-machines-windows-sizes.md) virtual machine käytettävissä koot luettelo.
    
3. Käyttöjärjestelmä AM asetusten määrittäminen. Tämä komento määrittää tietokonenimi, käyttöjärjestelmän tyyppi ja tilin tunnistetiedot AM.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Määritä kuva käyttämään valmistelu AM. Tämä komento määrittää AM käytettävät Windows Server-kuva. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Katso lisätietoja kuvien käyttäminen valitsemalla [Siirry ja valitse Windows virtuaalikoneen kuvat Azure PowerShell tai CLI](virtual-machines-windows-cli-ps-findimage.md).
        
5. Lisää verkkokäyttöliittymä, jonka loit työnkulkuun

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Määritä nimen ja sijainnin AM kiintolevyn. Virtuaalinen kiintolevyn-tiedosto on tallennettu säilön. Tämä komento luo levy-tallennustilan tilin, jonka loit **vhds/WindowsVMosDisk.vhd** säilö.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Lisää käyttöjärjestelmän levyn tiedot työnkulkuun AM. Korvaa **$diskName** arvosta käyttöjärjestelmä-levyn nimi. Luo muuttujan ja levy-tietojen lisääminen määritykset.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Luo lopuksi virtuaalikoneen.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Seuraavat vaiheet

- Jos käyttöönoton ongelmat, seuraava vaihe on tarkasteltavan [vianmääritys resurssin ryhmän ominaisuuksissa Azure-portaalissa](../resource-manager-troubleshoot-deployments-portal.md)
- Opi hallitsemaan virtuaalikoneen, jonka loit tarkastelemalla [hallinta näennäiskoneiden Azure Resurssienhallinta ja PowerShellin avulla](virtual-machines-windows-ps-manage.md).
- Voit hyödyntää mallin avulla voit luoda virtual machine edellä mainittujen tietojen avulla luominen [Windows-virtual machine Resurssienhallinta-malli](virtual-machines-windows-ps-template.md)
