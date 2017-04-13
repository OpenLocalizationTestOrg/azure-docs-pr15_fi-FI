<properties
    pageTitle="SQL Server Virtual kone luominen Azure PowerShell (perinteinen) | Microsoft Azure"
    description="Sisältää vaiheet ja PowerShell-komentosarjojen luomiseen Azure-AM SQL Serverin virtuaalikoneen valikoiman kuvia. Tässä ohjeaiheessa käyttää perinteinen käyttöönotto-tilaa."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Valmistele SQL Serverin virtual machine-PowerShellin Azure (perinteinen)

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa on ohjeet luoda SQL Server-virtual machine Azure käyttämällä PowerShell cmdlet-komentoja.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Katso tämän artikkelin Resurssienhallinta-versiota, [säännöstä SQL Serverin virtual machine-Azure PowerShell resurssien hallinnan avulla](virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Asenna ja määritä PowerShell:

1. Jos sinulla ei ole Azure-tili, käy [vapaa kokeiluversion Azure](https://azure.microsoft.com/pricing/free-trial/).

2. [Lataa ja asenna uusimmat Azure PowerShell-komennoilla](../powershell-install-configure.md).

3. Käynnistä Windows PowerShell ja yhdistä se Azure tilauksen **Lisää AzureAccount** -komennolla.

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Määrittää kohde Azure alue

SQL Server-virtuaalikoneen isännöityjen pilvipalvelussa, joka sijaitsee Azure tietyn alueen. Seuraavat vaiheet avulla voit määrittää alueen ja tallennustilaa tili ja cloud palvelu, jota käytetään opetusohjelman muiden.

1. Selvitä, jota haluat käyttää SQL Server-AM isännöimiseen tietokeskuksen. PowerShell-komennot näy käytettävissä alueiden tiedot, jossa yhteenveto luettelon lopussa.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Kun olet määrittänyt haluamasi sijainti, Määritä muuttujan **$dcLocation** , alue.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Tilauksen ja tallennustilaa tilin määrittäminen

1. Määritä uusi virtuaalikoneen käytettävän Azure tilaus.

        (Get-AzureSubscription).SubscriptionName

1. Määritä kohde Azure tilauksen **$subscr** muuttuja. Määritä tämä sitten Azure nykyisen tilauksesi.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Valitse olemassa olevan tallennustilan tileissä. Seuraavaa komentosarjaa näyttää kaikki tallennustilan tilit, jotka sijaitsevat valitun alueen:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Jos asetat uusi tallennustilan-tili, kuten seuraavassa esimerkissä uusi AzureStorageAccount-komennon Luo all--pieni tallennustilan tilin nimi: **Uusi AzureStorageAccount - StorageAccountName "<storage account name>"-sijaintiin $dcLocation**

1. Määritä **$staccount**kohde-tallennustilan tilin nimi. Valitse tilaus ja nykyistä tallennustilan tilin **Määrittäminen AzureSubscription** avulla.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Valitse SQL Server-virtuaalikoneen kuva

1. Lue luettelo käytettävissä SQL Server näennäiskoneiden kuvien valikoimasta. Kaikkien kuvien on **ImageFamily** -ominaisuus, joka alkaa seuraavasti: "SQL". Seuraava kysely näyttää käytettävissä kuva perheen käyttäjälle, joka on SQL Server esiasennettuna.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Kun olet löytänyt virtuaalikoneen kuva perheen, tämä tuoteperheeseen voi olla useita julkaistuja kuvia. Käyttämällä seuraavaa komentosarjaa valittu kuva perheen (kuten **SQL Server 2016 RTM Enterprise Windows Server 2012 R2-**) uusin julkaistu virtuaalikoneen kuvan nimi:

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>Luo virtuaalikoneen

Luo lopuksi virtuaalikoneen PowerShellin avulla:

1. Luo uusi AM isännöimiseen pilvipalveluun. Huomaa, että se on myös mahdollista käyttämään aiemmin pilvipalvelussa. Luo uusi muuttujan **$svcname** pilvipalvelussa sijaitsevaan lyhyt nimi.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Määritä virtuaalikoneen nimi ja koko. Saat lisätietoja virtuaalikoneen koosta [Virtuaalikoneen koot Azure](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Määritä paikallisen järjestelmänvalvojatili ja salasana.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Suorita seuraavaa komentosarjaa virtuaalikoneen luomiseen.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Lisää kuvaus ja asetukset- **komento joukon luominen** [Azure PowerShellin Luo ja määritä Windows-pohjaisesta näennäiskoneiden](virtual-machines-windows-classic-create-powershell.md)-kohdassa.

## <a name="example-powershell-script"></a>Esimerkki PowerShell-komentosarjaa

Seuraavaa komentosarjaa tarjoaa ja Esimerkki täydellisen komentosarjan, joka luo **SQL Server 2016 RTM Enterprise Windows Server 2012 R2-** virtual koneen. Jos käytät tätä komentosarjaa, mukautat alkuperäinen muuttujat edelliset vaiheet tämän aiheen perusteella.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Etätyöpöytä yhteydessä

1. Luo. Nykyisen käyttäjän tiedostokansio käynnistää nämä näennäiskoneiden viimeistelemään määritys RDP tiedostoja:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. Tiedostot-kansioon Käynnistä RDP-tiedostoon. Järjestelmänvalvojan käyttäjänimi ja salasana edellä yhteydessä (esimerkiksi jos käyttäjänimi on VMAdmin, Määritä "\VMAdmin" käyttäjänä ja annettava salasana).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Viimeistele määritys etäkäyttöpalvelimen SQL Server-konetta

Kun kirjaudutaan tietokoneessa, jossa etätyöpöydän kautta, Määritä SQL Server perusteella [seuraavien ohjelmien asetusten määrittämisestä SQL Server-yhteyksien Azure-AM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)ohjeita.

## <a name="next-steps"></a>Seuraavat vaiheet

Voit etsiä lisäohjeita valmistelu näennäiskoneiden PowerShellin [näennäiskoneiden ohjeissa](virtual-machines-windows-classic-create-powershell.md). SQL Server- ja Premium tallennustilan liittyviä lisätietoja komentosarjojen Katso [Käytä Azure Premium tallennustilan näennäiskoneiden SQL Serveriin](virtual-machines-windows-classic-sql-server-premium-storage.md).

Monissa tapauksissa seuraava vaihe on siirrettävä tietokantoja tämän uuden SQL Server-AM. Tietokannan siirtäminen ohjeita artikkelissa [siirtäminen SQL Server Azure-AM-tietokantaan](virtual-machines-windows-migrate-sql.md).

Jos olet myös käyttämällä luominen näennäiskoneiden SQL Azure-portaalin avulla, katso [valmistelu SQL Server Virtual Machine-Azure](virtual-machines-windows-portal-sql-server-provision.md). Huomaa, että opetusohjelman, joka opastaa sinua portaalin Luo VMs käyttää Resurssienhallinta suositellut malli käyttää PowerShell artikkelista perinteinen mallin sijaan.

Lisäksi seuraavia resursseja on suositeltavaa, että luet [muita aiheeseen liittyviä SQL Serveriä Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).
