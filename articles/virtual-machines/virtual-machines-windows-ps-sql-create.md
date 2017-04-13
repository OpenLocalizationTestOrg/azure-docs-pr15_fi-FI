<properties
    pageTitle="SQL Server Virtual kone luominen Azure PowerShell (Resurssienhallinta) | Microsoft Azure"
    description="Sisältää vaiheet ja PowerShell-komentosarjojen luomiseen Azure-AM SQL Serverin virtuaalikoneen valikoiman kuvia."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Valmistele SQL Serverin virtual machine-PowerShellin Azure (resurssien hallinta)

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShellin](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään, miten voit luoda yhteen Azure virtual tietokoneeseen **Azure Resurssienhallinta** käyttöönoton mallin avulla Azure PowerShellin cmdlet-komennot. Tässä opetusohjelmassa luodaan käyttämällä yhden levyaseman kuvan SQL-valikoiman yhteen virtual tietokoneeseen. Luodaan uusi tallennustilan, verkon ja Laske resurssit, jotka käyttävät virtuaalikoneen tarjoajat. Jos sinulla on jokin näistä resursseista aiemmin tarjoajien, voit käyttää tarjoajien.

Jos tarvitset tämän ohjeaiheen perinteisen version, katso [säännöstä SQL Server-virtual tietokoneeseen, käyttämällä Azure PowerShell perinteinen](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa, sinun on:

- Azure-tili ja tilauksen ennen kuin aloitat. Jos sinulla ei ole yhtä tilaamaan [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).
- [PowerShellin azure)](../powershell-install-configure.md)-version minimivaatimus 1.4.0 tai uudempi (Tässä opetusohjelmassa kirjoitettu versio 1.5.0).
    - Noutaa versiosi, kirjoita **Get-moduulin Azure ListAvailable**.

## <a name="configure-your-subscription"></a>Tilauksen määrittäminen

Avaa Windows PowerShellin ja muodostaa yhteyden Azure tilin suorittamalla seuraavat cmdlet-komento. Voit valita jommankumman ja kirjaudu sisään-näyttöön ja kirjoita tunnistetiedot. Käytä samaa sähköpostiosoitteella ja salasanalla, jonka avulla Kirjaudu Azure-portaaliin.

    Add-AzureRmAccount

Kun olet kirjautunut onnistuneesti näet tietoja näytöstä, joka sisältää SubscriptionId, joiden kanssa olet kirjautuneena sisään. Tämä on tilaus, johon tässä opetusohjelmassa resurssien luodaan, ellet muuta eri tilauksen. Jos sinulla on useita SubscriptionIds, suorita seuraava cmdlet-komento palauttaa luettelon kaikista oman SubscriptionIds:

    Get-AzureRmSubscription

Voit vaihtaa toiseen SubscriptionID, suorittamalla seuraavat cmdlet-komento haluamasi SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Määritä kuva muuttujat

Helpottaa käytettävyyttä ja ymmärtämisen Valmis komentosarja-Tässä opetusohjelmassa on Aloita määrittämällä muuttujien määrä. Muuta kaavarivillä parametriarvot sopivalta, mutta varo nimeämiskäytännön liittyvät nimen pituus ja erikoismerkkejä annettujen arvojen muokkaaminen.

### <a name="location-and-resource-group"></a>Sijainti ja resurssiryhmä
Kahden muuttujan avulla voit määrittää tietoalueen ja johon luot virtuaalikoneen muita resursseja resurssiryhmän.

Muokkaa haluamallasi tavalla ja suorita seuraavat cmdlet-komennot muuttujia alusta.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Tallennustilan ominaisuudet

Seuraavat muuttujat avulla voit määrittää tallennustilan tilin ja tallennustilaa käytettävän virtuaalikoneen tyyppi.

Muokkaa haluamallasi tavalla ja suorita sitten seuraavan cmdlet-komennon alustaa muuttujia. Huomaa, että tässä esimerkissä on käytössä [Premium tallennustilaa](../storage/storage-premium-storage.md), mikä on suositeltavaa tuotannon toiminnoista. Nämä ohjeet ja muut suositukset, artikkelissa [suorituskyvyn SQL Server Azuren näennäiskoneiden parhaat käytännöt](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Verkon ominaisuudet

Seuraavat muuttujat avulla voit määrittää verkkoliittymän, TCP/IP-kohdistusmenetelmä, VPN-nimen, virtual aliverkon nimi, virtual verkon alueen IP-osoitteiden, alueen IP-osoitteiden aliverkon ja virtuaalikoneen verkossa käytettävän julkisen toimialueen nimi-otsikko.

Muokkaa haluamallasi tavalla ja suorita sitten seuraavan cmdlet-komennon alustaa muuttujia.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Virtuaalikoneen ominaisuudet

Seuraavat muuttujat avulla voit määrittää virtuaalikoneen nimen, tietokonenimi, virtuaalikoneen kokoa ja virtuaalikoneen käyttöjärjestelmän-levyn nimi.

Muokkaa haluamallasi tavalla ja suorita sitten seuraavan cmdlet-komennon alustaa muuttujia.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Kuvan ominaisuudet

Seuraavat muuttujat avulla voit määrittää virtuaalikoneen käytettävä kuva. Tässä esimerkissä käytetään SQL Server 2016 Enterprise-kuva.

Muokkaa haluamallasi tavalla ja suorita sitten seuraavan cmdlet-komennon alustaa muuttujia.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Huomaa, että saat luettelon kaikista SQL Server kuva tarjouksia Get-AzureRmVMImageOffer-komennolla:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

Ja näet, tarjoaa Get-AzureRmVMImageSku-komennon käytettäväksi tuotteissa. Seuraava komento näkyy **SQL2014SP1 WS2012R2** saatavilla kaikissa tuotteissa tarjota.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Resurssiryhmän luominen

Resurssien hallinnan käyttöönotto-mallin ensimmäistä objektia, jonka luot on resurssiryhmän. Käytämme [Uusi AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) cmdlet-komento luominen Azure resurssiryhmä ja resursseja resurssiryhmän nimi ja sijainti määrittämiä muuttujat, jotka olet aiemmin alustaa.

Suorita seuraava cmdlet-komento luo uusi resurssiryhmä.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

Virtuaalikoneen edellyttää tallennustilan resurssien käyttöjärjestelmän levyn ja SQL Server-tietojen ja lokitiedostojen. Yksinkertaisuuden luodaan yhteen molemmat. Voit liittää muita levyjä sijoittaa SQL Server-tiedot ja erillinen levyille lokitiedostoja käyttämällä [Lisää Azure levyn](https://msdn.microsoft.com/library/azure/dn495252.aspx) cmdlet-komento. Käytämme [Uusi AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) cmdlet-komento vakio tallennustilan tilin luominen uusi resurssiryhmä ja tallennustilan tilin nimi, tallennustilan Sku nimi ja sijainti määritetty muuttujat, jotka olet aiemmin alustaa.

Suorita seuraavat cmdlet-komennolla voit luoda uuden tallennustilan tilin.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Luo verkkoresursseja

Virtuaalikoneen edellyttää useita verkkoresursseja verkkoyhteyden.

- Kunkin virtuaalikoneen edellyttää virtual verkkoon.
- Virtuaalinen verkko on määritetty vähintään yksi aliverkon.
- Verkkoliittymän on määritettävä julkinen tai yksityinen IP-osoite.

### <a name="create-a-virtual-network-subnet-configuration"></a>Luo VPN aliverkon asetukset

Olemme alkaa luomalla aliverkkomääritys, tutustu virtual verkossa. Tutustu opetusohjelmaan luodaan oletusarvon aliverkon [Uusi AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) cmdlet-komennolla. Olemme luodaan Microsoftin VPN aliverkon kokoonpanon aliverkon nimi ja osoite etuliite on määritetty käyttämällä muuttujat, jotka olet aiemmin alustaa.

>[AZURE.NOTE] Voit määrittää muita ominaisuuksia VPN-aliverkon määritys tätä cmdlet-komennolla, mutta se ei käsitellä tässä opetusohjelmassa.

Suorita seuraava cmdlet-komento luo virtuaalisia aliverkon järjestämällä.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Luo virtuaalisia verkko

Seuraavaksi luodaan Microsoftin virtual verkon [Uusi AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) cmdlet-komennolla. Tutustu virtual verkon luodaan uusi resurssiryhmä-, sisältää nimen, sijainnin ja osoitteen etuliite määritetään käyttämällä muuttujat, jotka olet aiemmin alustaa, ja aliverkon määritykset, jotka on määritetty edellisessä vaiheessa.

Suorita seuraava cmdlet-komento virtual verkoston luominen.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Luo julkinen IP-osoite

Nyt kun on määritetty Microsoftin virtual verkko-annettava virtuaalikoneen yhteys IP-osoitteen määrittäminen. Tässä opetusohjelmassa on luo julkisen IP-osoitteen käyttäminen dynaaminen IP-osoitteiden tukemaan Internet-yhteys. Käytämme [Uusi AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) cmdlet-komennolla voit luoda julkinen IP-osoite luodaan prevously resurssiryhmä ja sisältää nimen, sijainnin, kohdistusmenetelmä ja DNS-toimialueen nimi otsikko määritetty muuttujat, jotka olet aiemmin alustaa.

>[AZURE.NOTE] Voit määrittää julkisen IP-osoitteen käyttäminen cmdlet lisäominaisuudet, mutta tämä on ensimmäinen Tässä opetusohjelmassa laajemmin. Voit myös luoda yksityinen osoite tai osoite staattista osoitetta, mutta, joka on myös tässä opetusohjelmassa laajemmin.

Suorita seuraava cmdlet-komento luo julkinen IP-osoite.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Luo verkkokäyttöliittymä

On nyt valmis luomaan verkkokäyttöliittymä, joka käyttää Microsoftin virtuaalikoneen. Käytämme luominen resurssiryhmä, joka on luotu aiemmin ja sisältää nimen, sijainnin, aliverkon ja julkinen aiemmin määritetty IP-osoite sekä verkon-liittymän [Uusi AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) cmdlet-komento.

Suorita seuraavat cmdlet-komento luo verkon-liittymän.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Määritä AM-objekti

Nyt kun on määritetty säilytys- ja resursseja, emme valmis virtuaalikoneen Laske resurssien määrittäminen. Tässä opetusohjelmassa on Määritä virtuaalikoneen kokoa ja eri käyttöjärjestelmän ominaisuudet, Määritä verkkokäyttöliittymä, joka on aiemmin luotu, Määritä blob-säiliö ja määritä sitten käyttöjärjestelmän levy.

### <a name="create-the-vm-object"></a>AM-objektin luominen

Olemme alkaa määrittämällä virtuaalikoneen koon. Tässä opetusohjelmassa on ovat määrittäminen DS13. Käytämme [Uusi AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) cmdlet-komennolla voit luoda määritettäviä virtuaalikoneen objektin nimi ja määritetty käyttämällä muuttujat, jotka olet aiemmin alustaa koko.

Suorita seuraava cmdlet-komento virtuaalikoneen objektin luomiseen.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Luoda tunnistetiedon objektin ja pidä käyttäjänimen ja salasanan paikallisen järjestelmänvalvojan tunnistetiedot

Ennen kuin emme voi määrittää virtuaalikoneen käyttöjärjestelmän ominaisuudet-annettava Anna tunnistetiedot paikallisen järjestelmänvalvojan tilin suojatun merkkijonona. Tämän vuoksi Käytämme [Get tunnistetiedon](https://technet.microsoft.com/library/hh849815.aspx) cmdlet-komento.

Suorita seuraava cmdlet-komento ja Windows PowerShell-ikkunassa tunnistetiedon pyynnön Kirjoita nimi ja käyttämään Windows-virtuaalikoneen paikallisen järjestelmänvalvojan tilin salasana.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Määritä virtuaalikoneen käyttöjärjestelmän ominaisuudet

Nyt emme voi virtual machine käyttöjärjestelmän ominaisuuksien määrittäminen. Käytämme [Määrittäminen AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) cmdlet-komento määrittäminen Windows-käyttöjärjestelmän tyyppi, pyytää [virtuaalikoneen agentti](virtual-machines-windows-classic-agents-and-extensions.md) asenneta, Määritä cmdlet ottaa Automaattiset päivitykset ja virtuaalikoneen nimen, tietokonenimi ja käyttämällä muuttujat, jotka olet aiemmin alustaa tunnistetietojen määrittäminen.

Suorittaa seuraavan cmdlet-komennon avulla voidaan määrittää virtuaalikoneen käyttöjärjestelmän ominaisuudet.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Lisää verkkoliittymän virtuaalikoneen

Seuraavaksi on Lisää verkkokäyttöliittymä, joka on luotu aiemmin virtuaalikoneen. Käytämme [Lisää AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) cmdlet-komento lisää verkkoliittymän avulla verkko-liittymän muuttuja, joka aiemmin määritetty.

Suorita seuraavat cmdlet-komento virtuaalikoneen verkkoliittymän määrittämiseen.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Aseta levy käytettävän virtuaalikoneen blob tallennussijainnin

Seuraavaksi on määrittää Blob-objektien tallennuspaikka muuttujia, jotka olet määrittänyt aiemmin käyttämällä virtuaalikoneen käytettävän levyn.

Suorita seuraava cmdlet-komento blob storage sijainnin määrittäminen.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Määrittää käyttöjärjestelmän virtuaalikoneen levyn ominaisuudet

Olemme seuraavaksi määrittää käyttöjärjestelmän virtuaalikoneen levyn ominaisuudet. Käytämme [Määrittäminen AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) cmdlet-komennolla voit määrittää, että virtuaalikoneen käyttöjärjestelmän peräisin kuvan, voit määrittää välimuistiasetukset lukemaan vain (koska saman levyn asennetaan SQL Server) ja määritä virtuaalikoneen nimi ja käyttöjärjestelmän-levyn avulla muuttujat, jotka on määritetty aiemmin määritettyjä.

Suorittaa seuraavan cmdlet-komennon avulla voidaan määrittää käyttöjärjestelmän virtuaalikoneen levyn ominaisuudet.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Määritä virtuaalikoneen ympäristö kuva

Tutustu viimeinen määritysvaihe on Määritä Microsoftin virtuaalikoneen ympäristö kuva. Tässä opetusohjelmassa esimerkissä on käytössä uusimman SQL Server 2016 CTP kuva. Käytämme [Määrittäminen AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) cmdlet-komennon käyttäminen kuvassa määrittämät muuttujat, jotka aiemmin määritetty.

Suorita seuraava cmdlet-komento Määritä virtuaalikoneen ympäristö kuva.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>SQL-AM luominen

Nyt kun olet tehnyt määritysvaiheet, olet valmis luomaan virtuaalikoneen. Käytämme [Uusi AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) cmdlet-komento, joka on määrittänyt muuttujien käyttäminen virtuaalikoneen luomiseen.

Suorita seuraava cmdlet-komento luo virtuaalikoneen.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Virtuaalikoneen luodaan. Huomaa, että vakio tallennustilan-tili on luotu käynnistyksen diagnostiikan asetuksia, sillä virtual machine levyn määritetyn tallennustilan-tili on premium-tallennustilan tilin.

Nyt näet tämän tietokoneen Azure-portaalissa, näet [sen julkiseen IP-osoite ja sen täydellinen toimialuenimi](virtual-machines-windows-portal-sql-server-provision.md#Connect).  

## <a name="example-script"></a>Esimerkkikomentosarja

Seuraavaa komentosarjaa on valmis PowerShell-komentosarjaa Tässä opetusohjelmassa. Vaiheissa oletetaan, että sinulla on jo asennuksen Azure tilauksen **Lisää AzureRmAccount** ja **Valitse AzureRmSubscription** -komentojen käyttäminen.


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Seuraavat vaiheet
Kun virtuaalikoneen on luotu, olet valmis muodostaa virtuaalikoneen RDP ja määritys-yhteyden avulla. Lisätietoja on ohjeaiheessa [yhteyden, SQL Server Virtual tietokoneen Azure (resurssien hallinta)](virtual-machines-windows-sql-connect.md).
