<properties
    pageTitle="SQL Server Azure Premium tallennustilan käyttäminen | Microsoft Azure"
    description="Tässä artikkelissa käytetään resursseja perinteinen käyttöönotto-mallin avulla luotu ja antaa ohjeet Azure Premium tallennustilan käyttäminen-Azuren näennäiskoneiden SQL Serveriä."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Näennäiskoneiden SQL Server Azure Premium tallennustilan käyttäminen


## <a name="overview"></a>Yleiskatsaus

[Azure Premium tallennus](../storage/storage-premium-storage.md) on seuraavan sukupolven tallennustilaa, joka sisältää pienen viiveen ja suuri siirtonopeuden IO. Se toimii parhaiten avaimen IO tehostettu toiminnoista, kuten IaaS [näennäiskoneiden](https://azure.microsoft.com/services/virtual-machines/)SQL Server.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Tässä artikkelissa on suunnittelu ja ohjeita SQL Serveriä Premium tallennusvälineiden Virtual-tietokone siirtämistä varten. Tämä on Azure infrastruktuuri (verkko-tallennus) Vieras Windows AM. [Liitteen](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) esimerkissä koko täydellinen lopusta loppuun-siirto siirtämisestä suurempaa VMs hyödyntää parannetut paikallisen Suoritettaessa-tallennustilan PowerShellin avulla.

On tärkeää ymmärtää käyttävien Azure Premium tallennustilaa SQL Server-IAAS VMs lopusta loppuun-prosessin. Tämä vaihtoehto sisältää:

- Edellytykset Premium tallennusvälineiden tunnus.
- Esimerkkejä käyttöönotossa SQL Server-IaaS Premium tallennustilan uusi käyttöönotoissa.
- Esimerkkejä siirtäminen aiemmin käyttöönotoissa erillisissä palvelimissa ja ominaisuuksissa SQL aina käyttöön käytettävyys ryhmien käyttäminen.
- Mahdollisia siirron tavoista.
- Täysi lopusta loppuun-esimerkki, jossa näkyvät aiemmin aina toteutus siirron Azure, Windowsin ja SQL Serverin ohjeet.

Taustan Azuren näennäiskoneiden SQL Server-palvelimessa, katso lisätietoja [SQL Serverin Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).

**Tekijä:** Daniel Sol **tekniset tarkastajien:** Matti Teemu Vargas sillin, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Premium tallennustilan edellytyksistä

On useita edellytyksistä käyttämällä Premium-tallennustilan.

### <a name="machine-size"></a>Koneen kokoa

Ohjatun Premium tallennustilan sinun on käytettävä DS sarjan näennäiskoneiden (AM). Jos et ole käyttänyt DS sarjan koneet ennen oman pilvipalvelussa, poistaa aiemmin AM, pidä levyjen ja luo sitten uusi pilvipalvelussa ennen kuin DS * roolin koon AM luotaessa. Lisätietoja virtuaalikoneen koot artikkeleissa [virtuaalikoneen ja Azure Cloud palvelun koot](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Pilvipalveluihin

Käyttää vain sellaisia Premium tallennustilan DS * VMs luotaessa uusia pilvipalvelussa. Jos käytät SQL Server aina Azure-tietokannassa, aina käyttöön kuuntelua viittaa Azure sisäinen tai ulkoisen kuormituksen tasauspalvelun IP-osoitteeseen, joka on liitetty pilvipalveluun. Tässä artikkelissa keskitytään siirtäminen käytettävyys tässä skenaariossa säilyttäen.

> [AZURE.NOTE] Sarjan DS * on oltava ensimmäisen AM, jotka on otettu käyttöön uusi pilvipalveluun.

### <a name="regional-vnets"></a>Alueellisten VNETS

Hakemistopalvelun * VMs on määritettävä Virtual verkon (VNET) isännöinnin oman VMs on alueellisen. Tämä "väli"-VNET ansiosta suurempi VMs valmisteltava toisten klustereiden ja Salli viestintä niiden välille. Seuraavassa näyttökuvassa korostetun sijainti näkyy alueellisen VNETs ensimmäinen tulos näkyy "kapea" VNET.

![RegionalVNET][1]

Voit nostaa Microsoft-tuki lipun siirtämään alueellisen VNET, Microsoft muutosten tekeminen ja sitten Viimeistele alueellisen VNETs siirrosta muuttaa verkon määritysten AffinityGroup-ominaisuutta. Vie ensin verkon määritysten powershellissä ja **sijainti** -ominaisuuden korvaa **VirtualNetworkSite** osaan **AffinityGroup** -ominaisuus. Määritä `Location = XXXX` missä `XXXX` Azure alue. Valitse Tuo uudet määritykset.

Esimerkiksi lähitulevaisuudessa VNET asetukset ovat seuraavat:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Jos haluat siirtää tämä alueellisen VNET Länsi Euroopan, muuttaa määritykset seuraavasti:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Tallennustilan tilit

Haluat luoda uuden tallennustilan tilin, joka on määritetty Premium-tallennustilan. Huomaa, että Premium tallennustilan käyttö on määritetty tallennustilan tilin, ei yksittäisiä näennäiskiintolevyjen kuitenkin DS *-sarjan AM käytettäessä voit liittää Näennäiskiintolevyn päivän Premium ja tallennustilaa vakio-tileistä. Tämä kannattaa harkita, jos et halua OS Näennäiskiintolevyn voin Premium-tallennustilan tilin paikoilleen.

**Uusi AzureStorageAccountPowerShell** komentoriville "Premium_LRS" **tyyppi** Luo Premium-tallennustilan tilin:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Näennäiskiintolevyjen välimuistiasetukset

Tärkein luominen levyjä, jotka ovat osa Premium-tallennustilan tilin välinen ero on levyn välimuisti-asetus. SQL Server Data aseman levyjen on suositeltavaa, että käytät "**Luku välimuistiin**". Tapahtuman log levyasemien levyn välimuisti-asetus on asetettava**ei"**. Tämä on erilainen kuin Vakio tallennustilan tilit suosituksia.

Kun näennäiskiintolevyt liittänyt välimuisti-asetusta ei voi muuttaa. Haluat poistaa ja liittää uudelleen Näennäiskiintolevyn päivitetyt välimuisti-asetus.

### <a name="windows-storage-spaces"></a>Windows-tallennustilan välilyöntejä

Voit käyttää [Windows tallennustilan välilyöntejä](https://technet.microsoft.com/library/hh831739.aspx) ja edellisen vakio tallennustilan samoin kuin, tämän avulla voit siirtää AM, joka on jo käyttävien tallennustilan välilyöntejä. [Lisäys](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (vaihe 9 ja eteenpäin) esimerkissä purkaa ja tuoda useita liitetty näennäiskiintolevyjen kanssa AM Powershell-koodi.

Tallennustilan jakavat vieläkin vakio Azure-tallennustilan tilin parantaa siirtonopeuden ja vähentää viive. Voi olla arvo Premium tallennustilan ja tallennustilaa jakavat testaus uusi käyttöönotoissa, mutta he lisätä muita monimutkaisuus tallennustilan määritykset.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Tallennustilan jakavat Azure Virtual levyjen mikä kartta etsiminen

On liitetty näennäiskiintolevyjen eri välimuistin asetus suosituksia, saatat haluta kopioida näennäiskiintolevyt Premium-tallennustilan tilin. Kuitenkin liitä ne uudelleen uuden DS sarjan AM, kun haluat ehkä muuttaa välimuistiasetukset. On helpompaa, voit käyttää suositeltava välimuistiasetukset, kun sinulla on erillinen näennäiskiintolevyjen SQL-datatiedostot ja lokitiedostojen (sijaan yksittäisen Näennäiskiintolevyn, joka sisältää sekä) Premium-tallennustilan.

> [AZURE.NOTE] Jos sinulla on SQL Serverin tietojen ja lokitiedostojen samaan asemaan, voit valita välimuistiin vaihtoehto määräytyy IO access-kuvioiden määrittäminen tietokannan toiminnoista. Vain testaus voi osoittaa välimuistiin vaihtoehdon parhaiten Tämä skenaario.

Jos käytössäsi on Windows-tallennustilan tilat, jotka koostuvat haluat tarkastella useita näennäiskiintolevyjen alkuperäisen komentosarjojen tunnistamiseen, johon on liitetty näennäiskiintolevyjen on kuitenkin mitä tietyn varannon, niin voit määrittää välimuistiasetukset vastaavasti kunkin levylle.

Jos sinulla ei ole alkuperäisen komentosarjan voi näyttää, jossa näennäiskiintolevyjen yhdistäminen tallennustilan resurssivarantoon, voit seuraavia ohjeita määrittääksesi, levytilasta/resurssivarantoon yhdistäminen.

Käytä kunkin levyn seuraamalla näitä ohjeita:

1. Hae luetteloon liitetty AM **Get-AzureVM** -komennolla:

    Hae AzureVM - palvelun nimi <servicename> -nimen <vmname> | Hae AzureDataDisk

1. Huomautus Diskname ja LUN.

    ![DisknameAndLUN][2]

1. Etätyöpöytä kyselyjä AM. Siirry **Tietokoneenhallinta** | **Laitehallinta** | **levyasemien**. Tarkista kunkin 'Microsoft Virtual levyjen' ominaisuudet

    ![VirtualDiskProperties][3]

1. Tähän LUN arvo on viittaus LUN-numero, voit määrittää, kun Näennäiskiintolevyn liittäminen AM.
1. "Microsoft Virtual levyn" Valitse **tiedot** -välilehti, valitse **ominaisuus** -luettelossa Siirry **Ohjaimen avain**. **Arvo**Huomaa **Siirtymä**eli 0002 seuraavassa näyttökuvassa. 0002 ilmaisee PhysicalDisk2, joka viittaa tallennustilan resurssivarantoon.

    ![VirtualDiskPropertyDetails][4]

2. Tallennustilan kunkin ryhmän varten vedosta liittyvät levyjen:

    Hae StoragePool - FriendlyName AMS1pooldata | Get-fyysinen levy

    ![GetStoragePool][5]

Nyt voit käyttää näitä tietoja liitetään liittyvien näennäiskiintolevyjen tallennustilan jakavat fyysinen levyjen.

Kun olet yhdistänyt näennäiskiintolevyjen fyysinen levyille-tallennustilan jakavat, voit irrottaa ja kopioi ne Premium-tallennustilan tilin kautta, liitä ne oikein välimuisti-asetus. Katso [lisäys](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)Esimerkki vaiheet 8 – 12. Näitä ohjeita noudattamalla purkaa AM liitetty Näennäiskiintolevyn-levyn asetuksia CSV-tiedostoon, kopioi näennäiskiintolevyt, muuta levyn välimuistiasetukset ja ota lopuksi uudelleen AM DS-sarja AM kaikki levyjen.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>AM tallennustilan kaistanleveys ja Näennäiskiintolevyn tallennustilan siirtonopeuden

Tallennustilan suorituskyvyn määrä määräytyy määritetty DS * AM koko ja Näennäiskiintolevyn koot. VMs on eri korvauksia näennäiskiintolevyjen, joka voidaan liittää määrän ja suurin kaistanleveys ne tukee (Mt/s). Saat tietyt kaistanleveyden lukuja, [virtuaalikoneen ja Azure Cloud palvelun koot](virtual-machines-linux-sizes.md).

Parannettu IOPS saavutetaan suurempi levyn koot käyttämällä. Tämä kannattaa harkita, kun suunnittelet siirron polku. Lisätietoja on [IOPS ja levyn olevassa taulukossa](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Harkitse lopuksi VMs on eri enimmäismäärän vapaata kaistanleveyksien ne tukee kaikkia levyjä, jotka on liitetty. Valitse suuren kuormituksen aikana saattaa saturate enimmäismäärän vapaata kaistanleveyttä AM roolin kokoiset varten. Esimerkki Standard_DS14 tukee enintään 512 Mt/s; Tämän vuoksi kolme P30 levyä kanssa voi saturate AM levy-kaistanleveyden. Mutta tässä esimerkissä siirtonopeuden voitu saa ylittää pitämällä järjestelmässä sekä luku- ja kirjoitusoikeudet IOs mukaan.

## <a name="new-deployments"></a>Uusi ominaisuuksissa

Seuraavissa kahdessa osiossa näytetään, miten voit ottaa käyttöön Premium tallennustilan SQL Server VMs. Ennen mainittu ei välttämättä ole haluat sijoittaa OS levyn Premium tallennustilan sivulle. Voit halutessasi toiminto, jos ovat aikoo sijoittaa minkä tahansa tehostettu IO työmääriä OS ylläpito.

Ensimmäinen esimerkissä käyttävien aiemmin Azure-valikoiman kuvia. Toinen esimerkki näyttää mukautetun AM kuvan, joka on käytössä olevan tallennustilan vakio-tilin käyttämisestä.

> [AZURE.NOTE] Näissä Esimerkeissä oletetaan, että olet jo luonut alueellisen VNET.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Luo uusi AM Premium tallennustilan ja valikoiman kuva

Seuraavassa esimerkissä esitetään, kuinka OS Näennäiskiintolevyn premium tallennustilan sivulle ja liitä Premium tallennustilan näennäiskiintolevyjen. Voit kuitenkin sijoittaa myös OS levyn vakio tallennustilan tilin ja liitä sitten näennäiskiintolevyjen, jotka sijaitsevat Premium-tallennustilan tilin. Molemmat tilanteita, joissa on osoitettu.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Vaihe 1: Premium-tallennustilan tilin luominen


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Vaihe 2: Luo uusi pilvipalvelussa

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Vaihe 3: Varaa Cloud palvelun VIP (valinnainen)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Vaihe 4: Luominen AM säilö
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Vaihe 5: Sijoittaminen OS Näennäiskiintolevyn Standard tai Premium-tallennustilan
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Vaihe 6: Luo AM
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Luo uusi AM Premium tallennustilan käytettäväksi mukautetun kuvan

Tämä tilanne osoittaa, johon sinulla on aiemmin mukautettu kuvia, jotka sijaitsevat vakio tallennustilan tilillä. Kuten edellä, jos haluat sijoittaa OS Näennäiskiintolevyn Premium tallennustilan tarvitset Kopioi kuva, joka on vakio tallennustilan tilin ja siirtää ne Premium-tallennustilan, ennen kuin sitä voidaan käyttää. Jos sinulla on kuvan paikalliseen, voit käyttää myös tätä menetelmää voit kopioida, suoraan Premium-tallennustilan tilin.

#### <a name="step-1-create-storage-account"></a>Vaihe 1: Tallennustilan tilin luominen
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Vaihe 2 luominen pilvipalvelussa
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Vaihe 3: Käytä aiemmin luotua kuvaa
Voit käyttää aiemmin luodun kuvan. Vaihtoehtoisesti voit [ottaa aiemmin tietokoneen kuva](virtual-machines-windows-classic-capture-image.md). Huomautus Voit kuva ei tarvitse olla DS* koneen tietokoneeseen. Kun kuva, seuraavissa vaiheissa kopioiminen Premium-tallennustilan tilin, jolla * *Käynnistä AzureStorageBlobCopy** PowerShell-komentosovelmalla.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Vaihe 4: Kopioi Blob Storage tilien välillä
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Vaihe 5: Tarkista säännöllisesti kopioi tila seuraavasti:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Vaihe 6: Lisää kuva-levyn Azure levylle tilauksen säilöön
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Saatat huomata, että tilaraportit kuin onnistui, mutta voi vieläkään levy varauksen virhe. Tässä tapauksessa odota noin 10 minuuttia.

#### <a name="step-7--build-the-vm"></a>Vaihe 7: Muodosta AM
Seuraavassa on luomisesta AM kuva ja kaksi Premium tallennustilan näennäiskiintolevyjen liittäminen:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Olemassa olevan käyttöönotoissa, jotka eivät käytä aina käyttöön käytettävyys ryhmät

> [AZURE.NOTE] Aiemmin käyttöönottotilanteisiin on tämän artikkelin [edellytykset](#prerequisites-for-premium-storage) -osa.

Useilla eri huomioon otettavia seikkoja, jotka eivät käytä aina käyttöön käytettävyys ryhmät ja käyttäjät, joilla SQL Server-versioiden. Jos Käytä aina ja on olemassa erillinen SQL Server, voit päivittää Premium säilöön uuden cloud-palveluun ja tallennustilaa tilin avulla. Ota huomioon seuraavat vaihtoehdot:

- **Luo uusi SQL Server-AM**. Voit luoda uusi SQL-palvelimeen AM, joka käyttää Premium-tallennustilan tilin, uusi käyttöönotoissa suorittimessa. Valitse varmuuskopioiminen ja palauttaminen SQL Server-määritys ja käyttäjän tietokannat. Sovellus on päivitettävä viitteenä uusi SQL Server, jos se on käytössä sisälle tai. Haluat kopioida kaikki 'ulos db-objekteja, ikään kuin teit rinnakkain (SxS) SQL Server-siirron. Tämä sisältää objekteja, kuten kirjautumiset, varmenteet ja linkitettyjä palvelimia.
- **Siirrä aiemmin luotuun SQL Server-AM**. Tämä edellyttää ottaen SQL Server-AM offline-tilassa ja valitse siirtäminen uuteen pilvipalveluun, joka sisältää kaikki sen liitetty näennäiskiintolevyjen kopioiminen Premium-tallennustilan tilin. Kun AM kirjautuu-sovelluksen viitata server host nimi kuin ennen. Ota huomioon aiemmin levyn koko vaikuttaa suorituskykyominaisuudet. Jos esimerkiksi 400 gt DVD-levyllä saa pyöristää ñ20 =. Jos tiedät, että eivät edellytä, että suorituskyvyn, voit saattaa uudelleen nimellä DS-sarjan AM AM ja liittää Premium tallennustilan näennäiskiintolevyjen asetat koon ja suorituskyky-määrityksen. Voit sitten irrottaa ja liitä se uudelleen SQL-tietokantatiedostoja.

> [AZURE.NOTE] Kun sinun olisi otettava huomioon koon, sen mukaan, suuri Näennäiskiintolevyn levyjen kopioiminen tarkoittaa Premium tallennustilan levyn tyypin ne kuuluvat, määrittää levyn suorituskyvyn määritykset. Azure pyöreä ylöspäin lähimpään levyn kokoa, joten jos sinulla on 400 gt DVD-levyllä, tämä pyöristetään ñ20 =. Olemassa olevan IO tarpeidesi mukaan OS näennäiskiintolevyn ei joutua siirtäminen Premium-tallennustilan tilin.

Jos SQL Server käytetään ulkoisesti, cloud palvelun VIP muuttuu. Voit myös on päivitys päätepisteestä ja käyttöoikeusluettelot DNS asetukset.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Olemassa olevan käyttöönotoissa, joissa käytetään aina käyttöön käytettävyys ryhmät

> [AZURE.NOTE] Aiemmin käyttöönottotilanteisiin on tämän artikkelin [edellytykset](#prerequisites-for-premium-storage) -osa.

Alun perin tässä osassa on näyttää, miten aina toimii kanssa Azure verkko. Olemme vaihtuvat sitten alas siirtojen kaksi skenaarioiden: siirrot, joissa jotkin käyttökatkot voi esiintyä ja jossa on saavuttamiseksi mahdollisimman vähän käyttökatkot siirrot.

Paikallisen SQL Server aina käyttöön käytettävyys-ryhmien käyttäminen Listener paikallisen, joka rekisteröi virtual DNS-nimi sekä IP-osoite, joka on jaettu vähintään yksi SQL-palvelinten välillä. Kun asiakastietokoneet muodostavat yhteyden ne reititetään listener IP ensisijainen SQL Server –. Tämä on palvelin, joka omistaa aina käyttöön IP-resurssin aikalohkon.

![Valitse DeploymentsUseAlways][6]

Microsoft Azure voi olla vain yksi NIC-AM määritetty IP-osoite, valitse niin saavuttamiseksi samalle tasolle kuin paikallisen-artikkeli Azure käyttämällä IP-osoitetta, joka on määritetty sisäinen/ulkoinen kuormituksen tasoitusmääritykset (ILB/ELB). ILB/ELB kuin saman IP on määritetty IP-resurssi, joka on jaettu palvelinten välillä. Tämä on julkaistu DNS ja asiakkaan liikenne välitetään ensisijainen SQL Server-se ILB/ELB kautta. ILB/ELB tietää SQL Server on ensisijainen, koska se käyttää keräysputkien tutkia aina käyttöön IP-resurssi. Edellisessä esimerkissä se probes kunkin solmun, jossa on päätepisteen ELB/ILB viittaa, kumpi vastaa on ensisijainen SQL Server.

> [AZURE.NOTE] ILB ja ELB sekä määritetään tietyn Azure pilvipalveluun, sen vuoksi kaikki cloud siirron Azure-tietokannassa todennäköisesti tarkoittaa kuormituksen tasauspalvelun IP muuttuu.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Siirtäminen aina käyttöönotoissa, joka määrittää, että jotkin käyttökatkot

On kaksi strategioita siirtämään aina käyttöönotoissa, jotka sallivat joitakin käyttökatkot:

1. **Lisää toissijainen replikoiden lisääminen aiemmin luotuun aina-klusteriin**
1. **Uuden aina-klusterin siirtäminen**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. Lisää toissijainen replikoiden lisääminen aiemmin luotuun aina-klusteriin

Yksi vaihtoehto on Lisää Lisää secondaries aina käyttöön käytettävyys-ryhmään. Sinun täytyy lisätä ne uuteen pilvipalvelussa ja Päivitä kuuntelua uusi kuormituksen tasauspalvelun IP.

##### <a name="points-of-downtime"></a>Käyttökatkot kohtiin:

- Klusterin vahvistus.
- Testaus aina failovers uuden Secondaries.

Jos käytät Windows tallennustilan jakavat sisällä AM suurempi IO siirtonopeuden, valitse nämä otetaan offline-tilassa koko klusterin vahvistuksen aikana. Kelpoisuuden testi vaaditaan, kun lisäät solmujen klusterin. Testin suorittamiseen kuluvaa aikaa voi vaihdella, joten kannattaa testata edustavan testiympäristössä lähes ajan, ja kuinka kauan tämä kestää.

Aika, jossa voi tehdä manuaalinen automaattisesti ja testaus chaos aina käyttöön suuri käytettävyys Funktiot odotetulla tavalla, jotta juuri lisättyyn solmuissa pitäisi valmistella.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Kannattaa lopettaa kaikki esiintymät SQL Server tallennustilan jakavat käytettyjen ennen kelpoisuustarkistus toimii.
##### <a name="high-level-steps"></a>Ylätason vaiheita

1. Luo kaksi uudet SQL-palvelimet uusi pilvipalvelussa, joissa liitetyt Premium tallennusväline.
1. Kopioida koko varmuuskopiointi ja palauttaminen **NORECOVERY**kanssa.
1. Kopioi päälle 'ulos käyttäjällä DB' riippuvaiset objektit, kuten kirjautumiset jne.
1. Luo uusi uusi sisäinen kuormituksen tasauspalvelun (ILB) tai käytä ulkoisen kuormituksen tasauspalvelun (ELB) ja valitsemalla Määritä kuormituksen saapuva päätepisteet sekä uuden solmuissa.
> [AZURE.NOTE] Tarkista kaikki solmut on päätepisteen konfiguroinnin, ennen kuin jatkat

1. Lopettaa käyttäjän-sovellusta Access SQL Server (Jos käytössä tallennustilan jakavat).
1. Pysäytä ohjelma Servicesin kaikki solmut (Jos käytössä tallennustilan jakavat).
1. Lisää uusi solmujen klusterin ja suorita koko vahvistus.
1. Kun vahvistus on tarkistettu, Aloita kaikki SQL Server-palvelut.
1. Tapahtumalokit varmuuskopioiminen ja palauttaminen käyttäjän tietokannoissa.
1. Lisää uusi solmujen aina käyttöön käytettävyys ryhmään ja sijoita replikoinnin **synkroninen**kyselyjä.
1. Lisää IP-osoite-resurssi, uusi Cloud palvelun ILB/ELB PowerShellin kautta aina käyttöön usean sivuston esimerkki [lisäys](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)perusteella. Windowsin klusterointi arvoksi vanha uusien solmujen **mahdollisten omistajien** resurssin **IP-osoite** . 'Lisääminen IP osoite resurssin saman aliverkon' on kohdassa [lisäys](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
1. Siirtyy yhden uuden solmut automaattisesti.
1. Tee uuden solmujen automaattinen automaattisesti kumppanit ja testaa failovers.
1. Poistaa alkuperäisen solmujen käytettävyys-ryhmästä.

##### <a name="advantages"></a>Hyvät puolet

- Uudet SQL-palvelimet voidaan testata (SQL Server ja sovelluksen), ennen kuin ne lisätään aina.
- Voit AM koon muuttaminen ja mukauttaa tallennustilaan tarkka vaatimuksia. Kuitenkin olisi hyödyllisiin pitämään kaikki SQL tiedostopolkujen sama.
- Voit hallita, milloin toissijainen replikoiden DB-varmuuskopiot siirto on käynnistetty. Tämä eroaa Azure **Käynnistä AzureStorageBlobCopy** komentosovelmalla kopioiminen avulla näennäiskiintolevyjen, koska se on asynkroninen kopio.

##### <a name="disadvantages"></a>Huonot puolet
- Kun käytät Windowsin tallennustilan jakavat, on klusterin käyttökatkot koko klusterin kelpoisuuden, Lisää uusi solmujen.
- SQL Server-versio ja aiemmin määrä on toissijainen mukaan voit ei välttämättä voi lisääminen Lisää toissijainen replikoiden poistamatta aiemmin secondaries.
- Voi olla pitkä SQL-siirron aika aikana secondaries määrittäminen.
- On lisäkustannuksia siirron samalla, kun sinulla on uusi, joissa käytetään rinnakkain.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. siirtäminen uuteen aina-klusteriin

Toinen tapa on luoda uusi aina-klusterin uusi solmujen kanssa uusi pilvipalvelussa ja ohjaa asiakkaat voivat käyttää sitä.

##### <a name="points-of-downtime"></a>Käyttökatkot kohtiin

Ei käyttökatkot, kun siirrät sovellusten ja käyttäjien uusi aina kuuntelu. Käyttökatkot määräytyy:

- Palauta lopullinen tapahtuman Lokin varmuuskopioiden tietokantoja uusi palvelimissa aika.
- Päivitä asiakassovelluksissa käyttämään uusi aina kuuntelu aika.

##### <a name="advantages"></a>Hyvät puolet

- Voit testata todellinen tuotantoympäristössä SQL Server- ja käyttöjärjestelmän versio muutokset.
- Voit halutessasi tallennustilaa ja mahdollisesti AM pienentäminen. Tämä saattaa johtaa kustannukset vähentäminen.
- Voit päivittää SQL Server muodosta tai versio tämän prosessin aikana. Voit myös päivittää käyttöjärjestelmä.
- Edellisen aina-klusterin voi toimia tasainen palauttaminen kohde.

##### <a name="disadvantages"></a>Huonot puolet

- Sinun on muutettava kuuntelutoiminnon DNS-nimi, jos haluat, että molemmat aina-klustereiden samanaikainen. Tämä lisää asiakkaan sovelluksen merkkijonot on vastaavat Listener nimeämään hallinta yleisrasite siirron aikana.
- Synkronoinnin järjestelmä ympäristöissä pitämään ne mahdollisimman Pienennä lopullinen synkronoinnin vaatimukset ennen siirtoa lähellä välillä on pantava täytäntöön.
- Käytettävissä on lisätty kustannukset siirron aikana samalla, kun sinulla on käynnissä uuteen ympäristöön.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Siirtäminen aina-käyttöönotoissa mahdollisimman vähän käyttökatkot varten

Liittyy mahdollisimman vähän käyttökatkot siirtäminen aina ominaisuuksissa kaksi strategioita:

1. **Aiemmin toissijainen käyttämiseen: Single-sivusto**
1. **Aiemmin toissijainen Replica(s) käyttämiseen: usean sivuston**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. käyttämiseen aiemmin toissijainen: Single-sivusto

Yksi strategia mahdollisimman vähän käyttökatkot on aiemmin cloud toissijainen ja poistaa sen nykyisen pilvipalvelussa. Valitse Kopioi näennäiskiintolevyt uuden Premium-tallennustilan tilin ja luo uusi pilvipalvelussa AM. Päivitä kuuntelua klusterointi ja automaattisesti.

##### <a name="points-of-downtime"></a>Käyttökatkot kohtiin

- Tällä käyttökatkot, kun päivität solmun lopullinen kuormituksen saapuva päätepiste.
- Asiakkaan uuden-yhteyden voi viivästyä asiakkaan/DNS määritysten mukaan.
- Ei muita käyttökatkot, jos haluat ottaa aina-klusterin ryhmän offline-tilassa yhteystietokuvan IP-osoitteet. Voit välttää tämän käyttämällä tai riippuvuuden ja mahdollisten omistajien resurssille lisätty IP-osoite. 'Lisääminen IP osoite resurssin saman aliverkon' on kohdassa [lisäys](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).

> [AZURE.NOTE] Kun haluat lisätty solmu partake aina käyttöön automaattisesti kumppani, on lisättävä Azure päätepisteen, joka viittaa kuormituksen saapuva Set. Kun suoritat toiminto **Lisää AzureEndpoint** -komento, nykyisiä yhteyksiä kuuntelua Avaa, mutta uusi yhteydet säilyvät ole vahvistettava, ennen kuin kuormituksen on päivitetty. Testauksessa tämä on käytetty viimeisen 90-120seconds, tämä on testattava.

##### <a name="advantages"></a>Hyvät puolet

- Ei ole ylimääräisiä siirron aikana kertyneet kustannukset.
- Yksi-yhteen siirron.
- Rajoitetun monimutkaisuutta.
- Avulla saat paremman IOPS Premium tallennustilan tuotteissa. Kun levyjä on irrotettu AM ja kopioida uuteen pilvipalveluun 3rd osapuolen työkalun avulla voidaan Näennäiskiintolevyn kokoa, joka sisältää suurempi kierrosten. Lisääntyvien Näennäiskiintolevyn koot, katso [keskustelupalstalla keskustelun](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Huonot puolet

- Siirron aikana on HA ja DR tilapäisesti käytöstä.
- Tämä on 1:1-siirron, sinun on käytettävä, jotka tukevat sivunumeron näennäiskiintolevyjen, joten et välttämättä pysty downsize oman VMs AM vähimmäiskoko.
- Tässä skenaariossa käyttäisi Azure **Käynnistä AzureStorageBlobCopy** komentosovelmalla, joka on asynkroninen. Ei ole SLA kopioi valmiiksi. Kopiot aika vaihtelee samalla, kun tämän, riippuu siitä, odota jonossa se riippuu myös siirtää tietojen määrää. Kopioi aika kasvaa, jos siirto toiseen Azure tietokeskuksen, joka tukee Premium tallennustilan toisen alueen. Sinulla on 2 solmujen, harkitse mahdollista rajoituksen siltä varalta, että kopio kestää kauemmin kuin testauksessa. Tämä saattaa sisältää seuraavat ideoita.
    - Lisää väliaikaisen 3 SQL Server-solmu HA ennen siirtoa sovittuja käyttökatkot kanssa.
    - Suorita siirto ulkopuolelta Azure suunniteltu ylläpito.
    - Varmista klusterin quorum on määritetty oikein.  

##### <a name="high-level-steps"></a>Ylätason vaiheita

Tässä asiakirjassa ei näytetään valmis lopusta loppuun-esimerkki, mutta [Liite](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) sisältää tiedot, joita voidaan hyödyntää näin suoritettava.

![MinimalDowntime][8]

- Kerätä levyn asetuksia, sekä poistaa solmu (Älä poista liitetty näennäiskiintolevyjen).
- Premium-tallennustilan tilin luominen ja kopioi näennäiskiintolevyjen vakio tallennustilan tililtä
- Luo uusi pilvipalvelussa ja ota uudelleen SQL2 AM kyseisen pilvipalvelussa. Luo AM käyttämällä kopioidun alkuperäisen OS Näennäiskiintolevyn ja kopioitu näennäiskiintolevyjen liittäminen.
- Määritä ILB / ELB ja lisää päätepisteet.
- Päivittää Listener joko seuraavasti:
    - Tekeminen aina-ryhmässä offline-tilassa ja päivitetään aina käyttöön kuuntelua uusi ILB / ELB IP-osoite.
    - Tai lisäämällä IP address resurssi, uusi Cloud palvelun ILB/ELB PowerShellin kautta tuominen Windows klusterointi. Valitse määritetty IP-osoite resurssin mahdollisten omistajien siirretyt solmu SQL2, ja tekee tämä tai riippuvuus verkkonimi. 'Lisääminen IP osoite resurssin saman aliverkon' on kohdassa [lisäys](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
- Valitse DNS-määritys ja Välitystehtävän asiakkaille.
- Siirtää SQL1 AM ja käy läpi vaiheet 2 – 4.
- Jos käytät vaiheet 5ii, sitten Lisää SQL1 mahdollista omistajana lisätty IP-osoite resurssin
- Testaa failovers.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. käyttämiseen aiemmin toissijainen replica(s): usean sivuston

Jos sinulla on useampi kuin yksi Azure palvelinkeskuksen (Ohjauskoneen) solmujen tai yhdistelmäympäristön on, valitse voit aina-määritysten tässä ympäristössä minimoi käyttökatkot.

Vaihtoehto on muutettava synkroninen aina synkronointi paikallisen tai toissijaisen Azure Ohjauskoneen ja sitten automaattisesti välityksellä, SQL Server. Valitse Kopioi näennäiskiintolevyt Premium-tallennustilan tilin ja ota uudelleen koneen tuominen uuteen pilvipalvelussa. Päivitä kuuntelua ja epäonnistuu takaisin.

##### <a name="points-of-downtime"></a>Käyttökatkot kohtiin

Käyttökatkot koostuu aikaa, voit siirtää vaihtoehtoinen Ohjauskoneen ja takaisin. Se myös määräytyy asiakkaan/DNS-määrityksiä ja asiakkaan uuden-yhteyden saattaa viivästyä.
Esimerkki hybrid aina määrityksen:

![MultiSite1][9]

##### <a name="advantages"></a>Hyvät puolet

- Voit käyttää olemassa olevan infrastruktuurin.
- Voit halutessasi päivitystä edeltävä Azure tallennustilan DR Azure-Ohjauskoneen Valitse ensin.
- Ohjauskoneen DR Azure-tallennustilan määritettävä uudelleen.
- Siirron, lukuun ottamatta testi failovers on vähintään kaksi failovers.
- Sinun ei tarvitse SQL Serverin tietojen varmuuskopiointi ja palauttaminen.

##### <a name="disadvantages"></a>Huonot puolet

- Sen mukaan, SQL Server client access-saattaa olla parempi viive kun vaihtoehtoinen Ohjauskoneen on käynnissä SQL Server-sovellukseen.
- Kopioi keston näennäiskiintolevyjen Premium säilöön voi olla pitkä. Tämä voi olla vaikutusta päätöksesi käyttöön, säilytetäänkö solmu käytettävyys-ryhmässä. Ota huomioon, milloin log tehostettu työn Lataa käynnissä olevat siirron aikana on pakollinen, koska ensisijainen solmu on säilyttää sen tapahtumalokiin unreplicated tapahtumat. Tämän vuoksi tämä saattaa kasvaa merkittävästi.
- Tässä skenaariossa käyttäisi Azure **Käynnistä AzureStorageBlobCopy** komentosovelmalla, joka on asynkroninen. Ei ole SLA valmiiksi. Kopiot aika vaihtelee, kun tämän, riippuu siitä, odota jonossa, se riippuu myös siirtää tietojen määrää. Tämän vuoksi 2. tietokeskuksen on vain yksi solmu, olisi otettava vaiheet rajoituksen siltä varalta, että kopio kestää kauemmin kuin testauksessa. Tämä saattaa sisältää seuraavat ideoita.
    - Lisää väliaikaisen 2. SQL-solmu HA ennen siirtoa sovittuja käyttökatkot kanssa.
    - Suorita siirto ulkopuolelta Azure suunniteltu ylläpito.
    - Varmista klusterin quorum on määritetty oikein.

Tässä tilanteessa oletetaan, että sinulla on kuvattu asentamisessa ja tietää, kuinka tallennustilaan on yhdistetty, jotta voit tehdä muutoksia optimaalisen levyn välimuistin asetukset.

##### <a name="high-level-steps"></a>Ylätason vaiheita
![Multisite2][10]

- Varmista, paikallista / vaihtoehtoinen Azure Ohjauskoneen SQL Server-ensisijainen ja tekemällä siitä muut automaattinen automaattisesti kumppani (AFP).
- Levyn Hakumääritysten tietojen kerääminen SQL2 ja Poista solmu (Älä poista liitetty näennäiskiintolevyjen).
- Premium-tallennustilan tilin luominen ja kopioi näennäiskiintolevyjen vakio tallennustilan-tililtä.
- Luo uusi pilvipalveluun ja luoda SQL2 AM yhdistetty palkkioita tallennustilan sen levyjä.
- Määritä ILB / ELB ja lisää päätepisteet.
- Päivitä aina käyttöön kuuntelua uusi ILB / ELB IP-osoite ja testaa automaattisesti.
- Valitse DNS-määrityksiä.
- Muuta AFP SQL2, ja siirtää SQL1 ja käymällä läpi vaiheet 2 – 5.
- Testaa failovers.
- Siirry takaisin SQL1 ja SQL2 AFP

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Lisäys: Siirtyminen Multisite aina-klusterin Premium-tallennustilan

Tässä ohjeaiheessa loppuosa on esimerkki muuntaa usean sivuston aina klusterin Premium tallennustilan. Se myös muuntaa kuuntelua käyttämästä ulkoisen kuormituksen (ELB) sisäinen kuormituksen (ILB).

### <a name="environment"></a>Ympäristön

- Windowsin 2k 12 / 2k SQL 12
- Valitse SP 1 tietokantatiedostoja
- 2 x tallennustilan jakavat solmu kohden

![Appendix1][11]

### <a name="vm"></a>AM:

Tässä esimerkissä tarkastellaan osoittamaan siirtyvät ELB ILB. ELB ollut käytettävissä ennen ILB, joten tämä näyttää, miten voit siirtyä tähän siirron aikana.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Pre vaiheet: Tilauksen yhdistäminen

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Vaihe 1: Uuden tallennustilan asiakkaan ja Cloud palvelu
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Vaihe 2: Lisää resursseja sallitut virheet<Optional>
Tiettyjen aina käyttöön käytettävyys ryhmään kuuluvien resurssien, kuinka monta virheitä, joita voi syntyä piste, jossa klusteripalvelu yrittää uudelleen resurssiryhmän on rajoitukset. On suositeltavaa tämä suurentaa samalla, kun olet ovat ominaisuussäilöjen tämän toimintosarjan avulla jälkeen ellet manuaalisesti automaattisesti ja liipaisin failovers sulkemalla koneet, saa lähellä rajoitusta.

Se on järkevää kaksoisnapsauttamalla virheen alennuksen, voit tehdä tämän automaattisesti klusterin hallinnan Siirry aina-resurssiryhmä ominaisuudet:

![Appendix3][13]

Muuta suurin virheet 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Vaihe 3: Lisäys IP-osoite resurssin klusterin ryhmälle<Optional>

Jos käytössäsi on vain yksi IP-osoite klusterin-ryhmän ja tämä on tasattu cloud aliverkon, Varo, jos olet vahingossa offline-tilaan kaikki klusterin solmut pilveen verkon sitten klusterin IP-resurssi ja klusterin verkkonimi voi online-tilaan. Jos tämä estetään päivitykset klusterin muita resursseja.

#### <a name="step-4-dns-configuration"></a>Vaihe 4: DNS-määritys

Toteuttamisesta tasainen siirtymän riippuu siitä, miten DNS parhaillaan käytössä, ja päivitetään.
Aina on asennettu, se luo Windows klusterin-resurssiryhmä, jos olet avannut automaattisesti klusterin hallinta, näet, että vähintään se on kolme resurssia, joka viittaa asiakirjan kahden on:

- Virtuaalinen Network Name (VNN) – tämä on DNS-nimi, jota asiakas muodostetaan yhteys, kun taas kautta aina SQL-palvelimiin.
- IP-osoite-resurssi – tämä on VNN liittyvät IP-osoitetta, sinulla on useampi kuin yksi ja multisite määrityksessä voi esiintyä IP-osoite, sivuston/aliverkon kohden.

Yhteyden muodostaminen SQL Server, SQL Server Client ohjaimen noutaa kuuntelua liittyvän DNS-tietueet ja yritä muodostaa yhteys kunkin aina käyttöön liittyviä IP-osoite, kun alla käsiteltävät aiheet, jotka voivat vaikuttaa tämä tekijöitä.

Samanaikainen DNS-tietueet, jotka liittyvät listener nimi määrä riippuu ei vain numero IP-osoitteiden liittyviä, mutta '-vikasietoklustereihin aina edelleen VNN resurssin RegisterAllIpProviders'setting.

Kun otat käyttöön aina käyttöön Azure-tietokannassa on eri vaihetta Listener ja IP-osoitteet, sinun on määritettävä manuaalisesti 'RegisterAllIpProviders' 1, tämä on käytössä paikalliseen aina käyttöönoton eri, jossa on jo käytössä 1.

Jos 'RegisterAllIpProviders' on 0, näet vain yhden DNS-tietue DNS-liittyvät kuuntelua:

![Appendix4][14]

Jos 'RegisterAllIpProviders' 1:

![Appendix5][15]

Seuraava koodi tulevat vedosta VNN asetukset ja määritä se, Huomaa, jotta muutos tulee voimaan, sinun on VNN offline-tilaan ja ottaa sen takaisin online-tilaan tämän ottaen Listener offline-tilassa aiheuttaa asiakkaan connectivity häiriöt.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

Siirron myöhemmin, sinun on päivitettävä aina kuuntelua päivitetyt IP-osoitteeseen, joka viittaa kuormituksen Tämä edellyttää IP-osoite resurssin poistaminen ja lisääminen. IP-päivityksen jälkeen haluat varmistaa, uusi IP-osoite on päivitetty DNS-vyöhyke ja asiakkaiden päivittämässä niiden paikalliset DNS-välimuistin.

Jos asiakkaiden sijaitsevat eri verkko-osiossa ja viitata toiseen DNS-palvelimeen, sinun on ottaa huomioon, mitä tapahtuu tietoja DNS-vyöhyke siirtää siirron aikana, kun sovellus uudelleen kerran on rajoitettu mukaan vähintään uusi IP-osoitteita vyöhykkeen siirtäminen aika kuuntelua varten. Jos et valitse tähän aikarajoituksia, kannattaa keskustella ja testaa pakottaa vaiheittainen vyöhykkeen siirtäminen kanssa Windows-ryhmiä ja sijoittaa DNS-isännän tietue myös, pienempi aika, elinaika (TTL), niin asiakkaat päivittäminen. Lisätietoja on artikkelissa [Vaiheittainen vyöhykkeen siirrot](https://technet.microsoft.com/library/cc958973.aspx) ja [Käynnistä DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Oletusarvoisesti TTL DNS-tietueen, joka on liitetty kuuntelua aina olevalla Azure-tietokannassa on 1200 sekuntia. Halutessasi voit vähentää tämä, jos et valitse aika-rajoitus, jotta asiakkaiden Valmistele aikana päivittää niiden DNS kuuntelua päivitetyt IP-osoite. Voit tarkastella ja muokata määritykset polkumyynnin ulos VNN määrittäminen:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Huomaa, on pieni 'HostRecordTTL', DNS-liikenteen suurempi määrä tapahtuu.

##### <a name="client-application-settings"></a>Asiakkaan asetukset

Jos SQL-asiakassovellukseen tukee .net 4.5 SQLClient, voit käyttää ' MULTISUBNETFAILOVER = TRUE "-avainsana, tämä on suositeltavaa käytetään, kun se sallii nopeampaa yhteyden SQL aina käyttöön käytettävyys ryhmään automaattisesti aikana. Luetteloi kaikki IP-osoitteet, jotka liittyvät aina kuuntelua rinnakkain kautta, ja suorittaa Lisää kohdetoimialueen TCP uudelleen nopeus aikana automaattisesti.

Lisätietoja edellä asetuksista on artikkelissa [MultiSubnetFailover avainsana ja liittyvät toiminnot](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Katso myös [SqlClient tuki suuren käytettävyyden palauttaminen](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Vaihe 5: Klusterin quorum asetukset

Kun aiot ulos vähintään yksi SQL Server ottaen kerrallaan, kannattaa muokata klusterin quorum-asetus, jos tiedoston Jaa hallitun (FSW) käyttäminen 2 solmujen, sinun kannattaa asettaa vuokaavio, jotta suurimmalla solmu ja hyödyntää dynaaminen Äänestyksen ja tämä on yksittäinen solmu pysyvän pysyvän varten.


    Set-ClusterQuorum -NodeMajority  

Lisätietoja klusterin vuokaavio määrittäminen ja hallinta-kohdassa [Määritä ja Hallitse Windows Server 2012 automaattisesti klusterin vuokaavio](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Vaihe 6: Pura aiemmin päätepisteet ja käyttöoikeusluettelot
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Voit tallentaa ne tekstitiedostoon.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Vaihe 7: Muuttaminen automaattisesti kumppanit ja replikoinnin tilat

Jos sinulla on yli 2 SQL-palvelimet, kannattaa muuttaa toisen toissijainen toisen toimialueen Ohjauskoneen tai paikalliseen vikasietotilaa 'Synkroninen' ja ansiosta automaattinen automaattisesti kumppani (AFP), Näin voit säilyttää HA, kun teet muutoksia. Voit tehdä tämän joko TSQL vaikka SSMS muokkaaminen:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Vaihe 8: Poista toissijainen AM pilvipalvelussa

Voit olisi Haluatko siirtää pilveen toissijainen solmu ensimmäisen kerran, jos kyseessä on tällä hetkellä ensisijainen, kannattaa aloittaa manuaalinen automaattisesti.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Vaihe 9: Välimuistiasetukset CSV-tiedoston levyn muuttaminen ja tallentaminen

Tietomääriä, ne on määritettävä vain luku-TILASSA.

TLOG levyasemien nämä määritetään ei mitään.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Vaihe 10: Kopioi Näennäiskiintolevyjen
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Voit tarkistaa Premium tallennustilan tilille näennäiskiintolevyjen kopioi tila:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Odota, kunnes kaikki näihin kirjataan onnistui.

Lisätietoja yksittäisen BLOB:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Vaihe 11: Rekisteröi OS levy

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Vaihe 12: Tuoda toissijainen uusi pilvipalvelussa

Myös käyttää lisätty vaihtoehto alla olevan koodin Tässä voit tuoda koneen ja käyttää retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Vaihe 13: Luominen ILB uusi Cloud Svc Lisää kuormituksen saapuva päätepisteet ja käyttöoikeusluettelot
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Vaihe 14: Päivitä aina käyttöön
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Nyt voit poistaa vanhan pilvipalvelussa IP-osoite.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Vaihe 15: DNS-päivityksen tarkistaminen

Pitäisi nyt Tarkista DNS-palvelimet SQL Server client-verkoissa ja varmista, että klusterointi on lisätty lisätty IP-osoitetta ylimääräisiä Host (isäntä)-tietue. Jos nämä DNS-palvelimia ei ole päivitetty, voit pakottaa DNS-vyöhyke-siirron ja varmistaa, että asiakkaat siellä aliverkon voivat ratkaista sekä aina käyttöön IP-osoitteet, tämä on, joten sinun ei tarvitse odottaa, automaattinen DNS-replikoinnin.

#### <a name="step-16-reconfigure-always-on"></a>Vaihe 16: Uudelleen aina käyttöön

Tässä vaiheessa voit odottaa toissijaisen solmun, jotka on siirretty täysin Synkronoi kanssa paikallinen-solmu ja siirtyä synkronisen replikoinnin solmu ja ansiosta AFP.  

#### <a name="step-17-migrate-second-node"></a>Vaihe 17: Siirtää toisen solmu
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Vaihe 18: Välimuistiasetukset CSV-tiedoston levyn muuttaminen ja tallentaminen

Tietomääriä, ne on määritettävä vain luku-TILASSA.

TLOG levyasemien nämä määritetään ei mitään.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Vaihe 19: Luo uuden riippumaton tallennustilan tilin toissijainen solmu
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Vaihe 20: Kopioi Näennäiskiintolevyjen
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Voit tarkistaa kaikki näennäiskiintolevyjen Näennäiskiintolevyn kopioi tila: ForEach ($disk $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Levyn_nimi $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Odota, kunnes kaikki näihin kirjataan onnistui.

Lisätietoja yksittäisen BLOB: #Check induvidual blob tila Get-AzureStorageBlobCopyState-Blob-danRegSvcAms-dansqlams1-2014-07-03.vhd"-säilö $containerName-kontekstin $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Vaihe 21: Rekisteröi OS levy
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Vaihe 22: Lisää kuormituksen saapuva päätepisteet ja käyttöoikeusluettelot
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Vaihe 23: Testaa automaattisesti

Anna nyt siirretyt solmu aina solmu paikallinen synkronointi, aseta sen synkronisen replikoinnin tilaan ja odota, kunnes se on synkronoitu. Sitten ensimmäinen solmu-paikallinen automaattisesti uuteen, joka on AFP. Kun, joka on tehnyt, muuta viimeinen siirretyt solmu AFP.

Kannattaa testata failovers kaikki solmujen välillä ja suorita mutta chaos testien varmistamiseksi failovers työn oikein ja julkaisemiseen manor.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Vaihe 24: Takaisin klusterin quorum asetukset / DNS TTL / automaattisesti Pntrs tai synkronointiasetusten
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Lisäämällä saman aliverkon IP-osoite-resurssi

Jos on vain 2 SQL-palvelimia ja haluat lisätä uuden pilvipalveluun, mutta haluat säilyttää ne saman aliverkon, voit välttää ottaen kuuntelua offline-tilassa alkuperäisen aina käyttöön IP-osoite ja Lisää uusi IP-osoite. Jos olet siirtymässä VMs toiseen aliverkon tarvitset ei tee näin ole muita klusterin-verkkoon, joka viittaa kyseisen aliverkon.

Kun olet tuodut siirretyt toissijaisen ylöspäin ja lisätty uusi IP-osoite resurssille automaattisesti aiemmin ensisijainen ennen uuden pilvipalvelussa, pitäisi toimia seuraavasti klusterin automaattisesti hallinta-alueella:

Voit lisätä IP-osoite-kohdassa [lisäys](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)vaihe 14.

1. Nykyisen IP-osoite, resurssin alla olevasta esimerkistä 'dansqlams4' 'aiemmin ensisijainen SQL Server-, mahdollista omistajan muuttaminen:

    ![Appendix13][23]

1. Uuden IP-osoite, resurssin 'Migrated toissijainen SQL Server-, alla olevasta esimerkistä 'dansqlams5' mahdollista omistajan muuttaminen:

    ![Appendix14][24]

1. Kun tämä on määritetty voit automaattisesti, ja kun viimeinen solmu siirretään mahdollisten omistajien kannattaa muokata, jotta solmun lisätään mahdollista omistaja:

    ![Appendix15][25]

## <a name="additional-resources"></a>Lisäresursseja
- [Azure Premium-tallennustilan](../storage/storage-premium-storage.md)
- [Näennäiskoneiden](https://azure.microsoft.com/services/virtual-machines/)
- [SQL Server Azure-Virtuaalikoneissa](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
