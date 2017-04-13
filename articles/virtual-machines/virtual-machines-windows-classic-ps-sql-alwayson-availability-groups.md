<properties
    pageTitle="Määritä aina käytettävyys ryhmän Azure AM PowerShellin avulla"
    description="Tässä opetusohjelmassa käytetään resursseja perinteinen käyttöönotto-mallin avulla luotu ja käyttää PowerShell aina käyttöön käytettävyys-ryhmän luominen Azure-tietokannassa."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Määritä aina käytettävyys ryhmän Azure AM PowerShellin avulla

> [AZURE.SELECTOR]
- [Resurssienhallinta: malli](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Resurssienhallinta: Manuaalinen](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Perinteinen: Käyttöliittymä](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Perinteinen: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure-virtuaalikoneissa (VMs) auttaa tietokannan järjestelmänvalvojat voivat toteuttaa pienempi suuren käytettävyyden SQL Server-järjestelmän kustannukset. Tässä opetusohjelmassa näytetään ottamisesta käyttöön käyttämällä SQL Server aina käyttöön-kattavan sisällä Azure ympäristössä käytettävyys-ryhmä. Opetusohjelman lopussa Azure SQL Server aina-ratkaisu sisältää seuraavat osat:

- Virtuaalinen verkkoon, joka sisältää useita aliverkosta, mukaan lukien edusta- ja taustatietokantaan aliverkon

- Toimialueen ohjauskoneen Active Directory (AD)-toimialueella

- Kaksi SQL Server VMs käyttöön taustatietokantaan aliverkon ja AD-toimialueen

- 3-solmu WSFC klusterin solmu suurimmalla quorum-malli

- Käytettävyys ryhmä, josta on synkronoitu Vahvista replikat käytettävyys tietokanta

Tässä skenaariossa valitaan sen yksinkertaisuuden ei sen kustannukset tehokkuuden tai muiden tekijöiden Azure varten. Voit esimerkiksi pienentää VMs kaksi replikan käytettävyys ryhmän määrän tallentamiseksi Laske tunnit Azure käyttämällä toimialueen ohjauskoneen 2-solmu WSFC klusterin quorum tiedoston jakaminen-hallitun. Tätä tapaa vähentää AM Laske jollakin edellä asetuksista.

Tässä opetusohjelmassa eivät Näytä toimet, joita tarvitaan määritetään kuvattuja ratkaisu ilman laadittaessa Valitse jokaisen vaiheen tiedot. Tämän vuoksi sijaan näytetään Graafisen määritysvaiheet, se käyttää PowerShell-komentosarjojen, jolla pääset nopeasti kaikki vaiheet. Oletetaan seuraavasti:

- Sinulla on jo Azure-tili ja virtuaalikoneen-tilaus.

- Olet asentanut [Azure PowerShellin cmdlet-komennot](../powershell-install-configure.md).

- Sinulla on jo tasainen tietoja aina käyttöön käytettävyys ryhmien paikallinen ratkaisuja. Lisätietoja on kohdassa [Aina käyttöön käytettävyys Groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Muodosta yhteys Azure-tilauksesi ja Virtual verkoston luominen

1. Paikallisessa tietokoneessa PowerShell-ikkunassa tuo Azure moduuli, lataa julkaisun asetustiedosto tietokoneeseesi ja Yhdistä PowerShell-istunnon Azure tilauksen tuomalla ladattujen julkaisun asetukset.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    **Hae AzurePublishgSettingsFile** -komento luo automaattisesti hallinnan varmenteen Azure ja lataa se tietokoneeseesi. Selain avautuu automaattisesti ja sinua pyydetään antamaan Azure tilauksen Microsoft-tilin tunnistetiedot. Ladatun **.publishsettings** -tiedosto sisältää kaikki haluamasi tiedot, sinun on hallittava Azure tilauksen. Kun olet tallentanut tiedoston paikallisessa kansiossa, tuo se **Tuo AzurePublishSettingsFile** -komennon avulla.

    >[AZURE.NOTE] Publishsettings-tiedosto sisältää tunnistetiedot (koodaamaton), joiden avulla voit hallita Azure tilaukset ja palveluja. Suojauksen tämän tiedoston parhaat käytännöt kannattaa tallentaa sen tilapäisesti lähde-kansioiden (kuten kansiossa Libraries\Documents) ulkopuolella ja poistaa sen, kun tuonti on valmis. Haittaohjelmien käyttäjän luokitus käyttää publishsettings-tiedoston voi muokata, luominen ja poistaminen Azure palvelujen.

1. Määritä muuttujat, jotka voit luoda oman cloud IT-infrastruktuurin sarjaa.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Kiinnitä huomiota varmistaa, että komennot onnistuu myöhemmin seuraavasti:

    - Muuttujat **$storageAccountName** ja **$dcServiceName** on oltava yksilöllinen koska niitä käytetään tunnistavan cloud tallennustilan tilin ja pilvi palvelimesi vastaavasti Internetissä.

    - Muuttujat **$affinityGroupName** ja **$virtualNetworkName** määritetyt nimet on määritetty VPN-määritys asiakirja, jota käytät myöhemmin.

    - **$sqlImageName** määrittää päivitetyt AM-kuva, joka sisältää SQL Server 2012 Service Pack 1 Enterprise Edition.

    - Yksinkertaisuuden **Contoso! 000** on sama koko koko opetusohjelman käytetyt salasana.

1. Luo affiniteetti-ryhmä.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Luo virtuaalisia verkon tuomalla määritystiedostoa.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Määritystiedosto sisältää seuraavat XML-asiakirja. Lyhyesti se määrittää kutsutaan **ContosoNET** affiniteetti ryhmässä **ContosoAG**virtual verkon ja on osoite tilaa **10.10.0.0/16** ja on kahden aliverkon, **10.10.1.0/24** ja **10.10.2.0/24**, jotka ovat eteen aliverkon ja takaisin aliverkon tarpeen mukaan. Postikortin etupuoli aliverkon on, jossa voit sijoittaa asiakassovellusten, kuten Microsoft SharePoint ja takaisin aliverkon on kohtaa, johon siirrät SQL Server-VMs. Jos muutat **$affinityGroupName** ja **$virtualNetworkName** muuttujat aikaisemmin, sinun on muutettava myös alla vastaavat nimet.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Luo tallennustilan-tili, joka on liitetty affiniteetti ryhmän luotu ja tekee siitä nykyistä tallennustilan tilin-tilaukseesi.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Luo Ohjauskoneen palvelimeen uuden cloud-palvelun ja käytettävyyden määrittäminen.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Tämä videosarja siirtoa putkissa komentoja, tee seuraavat kohdat:

    - **Uusi AzureVMConfig** Luo AM-määritykset.

    - **Lisää AzureProvisioningConfig** antaa on erillinen Windows server configuration-parametrit.

    - **Lisää AzureDataDisk** Lisää tietoja-levyn, jota voit käyttää projektiin Active Directory-tiedot, jossa välimuisti ei mitään.

    - **Uusi AzureVM** Luo uusi pilvipalvelussa ja uusi Azure AM uusi pilvipalvelussa.

1. Odota, että uusi AM valmisteltava täysin ja lataa Etätyöpöytä tiedoston toimimasta-kansioon. Uusi Azure AM kestää kauan valmistelu, koska hetken silmukka säilyy äänestyksen järjestäminen uusi AM, kunnes se on valmis käytettäväksi.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Toimialueen Ohjauskoneen palvelin on nyt valmisteltu. Seuraavaksi voit määrittää Active Directory-toimialueen Ohjauskoneen tähän palvelimeen. PowerShell-ikkunan jättäminen auki paikalliseen tietokoneeseen. Uudelleen käytetään kahta SQL Server-VMs luominen.

## <a name="configure-the-domain-controller"></a>Toimialueen ohjauskoneen määrittäminen

1. Etätyöpöytä tiedoston avaamista yhdistäminen Ohjauskoneen palvelimeen. Tietokoneen järjestelmänvalvojan AzureAdmin käyttäjänimi ja salasana **Contoso! 000**, kun luot uuden AM määritettyä.

1. Avaa PowerShell-ikkuna järjestelmänvalvojan oikeuksia.

1. Suorita seuraava **DCPROMO. EXE** asennuksen **corp.contoso.com** toimialueen tietojen kansioita asemassa M-komennolla.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Kun komento on valmis, AM käynnistyy automaattisesti.

1. Yhdistäminen Ohjauskoneen palvelimeen uudelleen etätyöpöytä tiedoston avaamista. Tällä hetkellä Kirjaudu sisään **CORP\Administrator**.

1. Avaa PowerShell-ikkuna järjestelmänvalvojan oikeuksin ja tuo PowerShellin Active Directory-moduulin käyttämällä seuraava komento:

        Import-Module ActiveDirectory

1. Suorita seuraavat komennot kolme käyttäjien lisääminen toimialueelle.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** käytetään määrittämään kaikki liittyvät SQL Server-palvelun esiintymät, WSFC-klusterin ja käytettävyys-ryhmä. **CORP\SQLSvc1** ja **CORP\SQLSvc2** käytetään SQL Server service-tileillä kaksi SQL Server-VMs.

1. Seuraavaksi suorittamalla seuraavat komennot ja anna **CORP\Install** tietokoneobjektien luomiseen toimialueen käyttöoikeudet.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Yllä mainittu GUID-tunnus on tietokoneen objektilaji GUID-tunnus. **CORP\Install** -tili on **Luku kaikki ominaisuudet** ja **Luo tietokoneen objektit** -käyttöoikeudet, jotta aktiivinen suoraan objektien WSFC-klusterin luominen. **Luku kaikki ominaisuudet** -käyttöoikeus on jo annettu CORP\Install oletusarvoisesti, jotta sinun ei tarvitse myöntää eksplisiittisesti. Katso lisätietoja WSFC-klusterin luomiseen tarvittavia oikeuksia [automaattisesti klusterin vaiheittainen opas: Active Directory-tilit määrittäminen](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Nyt kun olet määrittäminen Active Directory- ja käyttäjä-objektit, luodaan kahden SQL Server VMs ja liittäminen toimialueessa.

## <a name="create-the-sql-server-vms"></a>SQL Server-VMs luominen

1. Jatkaa PowerShell-ikkuna, jossa on avoinna omassa tietokoneessasi. Määritä seuraavat muita muuttujat:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    IP-osoite **10.10.0.4** määritetään yleensä ensimmäisen AM **10.10.0.0/16** aliverkon Azure virtual verkoston luominen. Tarkista tämä on Palvelinkeskus-palvelimen osoite suorittamalla **IPCONFIG**.

1. Suorita ensimmäisen AM luominen WSFC-klusterin nimeltä **ContosoQuorum**siirtoa putkissa seuraavia komentoja:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Yllä olevien komentojen koskevat seuraavat seikat:

    - **Uusi AzureVMConfig** Luo AM määrityksen, jossa haluamasi käytettävyys joukon nimi. Seuraavien VMs luodaan samannimistä käytettävyys määrittäminen niin, että ne liitetään käytettävyys samat.

    - **Lisää AzureProvisioningConfig** yhdistää AM luomasi Active Directory-toimialueen.

    - **Määritä AzureSubnet** sijoittaa AM Edellinen aliverkon.

    - **Uusi AzureVM** Luo uusi pilvipalvelussa ja uusi Azure AM uusi pilvipalvelussa. **DnsSettings** -parametri ilmaisee uuden pilvipalvelussa palvelimia DNS-palvelin on IP osoite **10.10.0.4**, joka on Ohjauskoneen palvelimen IP-osoite. Tämä parametri tarvita käyttöön uuden VMs pilvipalvelussa liittäminen Active Directory-toimialueen onnistuneesti. Tämä parametri ilman IPv4-asetusten määrittämisestä oman AM käytettävä Ohjauskoneen palvelimen ensisijainen DNS-palvelin jälkeen AM on valmisteltu manuaalisesti ja sitten liittää AM Active Directory-toimialueen.

1. Suorita seuraavat siirtoa putkissa komennot SQL Server VMs **ContosoSQL1** ja **ContosoSQL2**luomiseen.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Yllä olevien komentojen koskevat seuraavat seikat:

    - **Uusi AzureVMConfig** käyttää samaa käytettävyys joukon nimi Ohjauskoneen palvelimeen, ja käyttää SQL Server 2012 Service Pack 1 Enterprise Edition-kuva virtuaalikoneen-valikoimassa. Se myös määrittää käyttöjärjestelmän levy, luku-välimuistin vain (ei kirjoittaminen välimuisti). On suositeltavaa siirtää tietokantatiedostoja erillinen tietojen levylle AM liittäminen ja määritä se ei ole luku tai kirjoittaa välimuistiin. Melkein on kuitenkin poistaa kirjoittaminen välimuistiin käyttöjärjestelmän levyllä, koska et voi poistaa luetuksi käyttöjärjestelmän levyn välimuistiin.

    - **Lisää AzureProvisioningConfig** yhdistää AM luomasi Active Directory-toimialueen.

    - **Määritä AzureSubnet** sijoittaa AM takaisin aliverkon.

    - **Lisää AzureEndpoint** Lisää access päätepisteet niin, että asiakassovellukset voivat käyttää näitä SQL Server-palveluiden esiintymien Internetissä. Eri portit annetaan ContosoSQL1 ja ContosoSQL2.

    - **Uusi AzureVM** Luo uusi SQL Server-AM saman pilvipalvelussa ContosoQuorum nimellä. Sinun täytyy lisätä VMs saman pilvipalvelussa, jos haluat niiden olevan käytettävyys samat.

1. Odota, että kunkin AM valmisteltava täysin ja ladata sen Etätyöpöytä tiedoston toimimasta-kansioon. -Silmukan käy läpi kolme uusi VMs ja suorittaa komennot ylimmän tason Kaarevien hakasulkeiden sisällä kullekin niistä.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    SQL Server-VMs nyt valmisteltu ja käytössä, mutta ne asennetaan SQL Serverin kanssa oletusasetukset.

## <a name="initialize-the-wsfc-cluster-vms"></a>WSFC klusterin VMs alusta

Tässä osassa haluat muokata kolme palvelinta aiot käyttää WSFC-klusterin ja SQL Server-asennus. Tarkemmin:

- (Jokaisessa palvelimessa) Sinun täytyy asentaa **Vikasietoklusterointia** .

- (Jokaisessa palvelimessa) Sinun on lisättävä **CORP\Install** tietokoneen **järjestelmänvalvoja**.

- (ContosoSQL1 ja vain ContosoSQL2) Haluat lisätä **CORP\Install** **järjestelmänvalvojaryhmään oletusarvo-tietokannassa** .

- (ContosoSQL1 ja vain ContosoSQL2) Sinun on lisättävä **NT AUTHORITY\System** kuin kirjautuminen seuraavat käyttöoikeudet:

    - Muuta käytettävyys ryhmälle

    - Yhteyden SQL

    - Näytä palvelimen tila

- (ContosoSQL1 ja vain ContosoSQL2) SQL Server-AM jo käytössä **TCP** -protokollaa. Kuitenkin yhä haluat avata palomuuri SQL Serverin etäkäyttöä varten.

Nyt olet valmis aloittamaan. Alkavat **ContosoQuorum**, noudata seuraavia ohjeita:

1. Muodosta yhteys **ContosoQuorum** remote työpöydän tiedostojen avaamista. Tietokoneen järjestelmänvalvojan **AzureAdmin** käyttäjänimi ja salasana **Contoso! 000**, määritettyä VMs luotaessa.

1. Varmista, että tietokoneet on liitetty onnistuneesti **corp.contoso.com**.

1. Odota loppuun, ennen kuin jatkat Automaattinen alustus-tehtävien suorittaminen SQL Server-asennuksen.

1. Avaa PowerShell-ikkuna järjestelmänvalvojan oikeuksia.

1. Asenna Windows Vikasietoklusterointia.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Lisää **CORP\Install** paikallisen järjestelmänvalvojan.

        net localgroup administrators "CORP\Install" /Add

1. Kirjaudu ulos ContosoQuorum. Nyt on tehty tämän palvelimen kanssa.

        logoff.exe

Seuraavaksi alusta **ContosoSQL1** ja **ContosoSQL2**. Noudata seuraavia ohjeita, jotka ovat samat molemmissa SQL Server-VMs.

1. Muodosta yhteys kaksi SQL Server-VMs remote työpöydän tiedostojen avaamista. Tietokoneen järjestelmänvalvojan **AzureAdmin** käyttäjänimi ja salasana **Contoso! 000**, määritettyä VMs luotaessa.

1. Varmista, että tietokoneet on liitetty onnistuneesti **corp.contoso.com**.

1. Odota loppuun, ennen kuin jatkat Automaattinen alustus-tehtävien suorittaminen SQL Server-asennuksen.

1. Avaa PowerShell-ikkuna järjestelmänvalvojan oikeuksia.

1. Asenna Windows Vikasietoklusterointia.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Lisää **CORP\Install** paikallisen järjestelmänvalvojaksi

        net localgroup administrators "CORP\Install" /Add

1. Tuo SQL Server PowerShell-palvelu.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Lisää **CORP\Install** SQL Server oletusesiintymää järjestelmänvalvojaryhmään.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. Lisää **NT AUTHORITY\System** kirjautuminen, jossa on kolme edellä kuvatun käyttöoikeudet.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Avaa palomuurin SQL Serverin etäkäyttöä varten.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Kirjaudu ulos sekä VMs.

        logoff.exe

Lopuksi olet valmis käytettävyys-ryhmän. Suorittaa kaikkia töitä **ContosoSQL1**SQL Server PowerShell-palvelun avulla.

## <a name="configure-the-availability-group"></a>Määritä käytettävyys-ryhmä

1. Muodosta yhteys **ContosoSQL1** uudelleen remote työpöydän tiedostojen avaamista. Sen sijaan, että kirjautuminen käyttämällä koneen tili, kirjaudu sisään **CORP\Install**.

1. Avaa PowerShell-ikkuna järjestelmänvalvojan oikeuksia.

1. Määritä seuraavat muuttujat:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. Tuo SQL Server PowerShell-palvelu.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Muutettava SQL Server-tilin ContosoSQL1 CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Muutettava SQL Server-tilin ContosoSQL2 CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Lataa **CreateAzureFailoverCluster.ps1** [Aina käyttöön käytettävyys ryhmien Azure AM WSFC klusterin luominen](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) paikalliseen toimimasta-kansioon. Käytät tätä komentosarjaa avulla voit luoda toimintojen WSFC-klusterin. Tärkeitä tietoja siitä, miten WSFC toimii Azure verkoston kanssa on artikkelissa [suuren käytettävyyden ja SQL Server Azuren näennäiskoneiden palauttaminen](virtual-machines-windows-sql-high-availability-dr.md).

1. Siirry toimimasta-kansioon ja luo WSFC-klusterin ladattujen komentosarjan.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Aina käyttöön käytettävyys ryhmien käyttöön **ContosoSQL1** ja **ContosoSQL2**oletusarvon SQL Server-esiintymät.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Luo varmuuskopio-kansio ja SQL Server-palvelutilejä käyttöoikeuksia. Toissijainen se käytettävyys-tietokannan valmisteleminen tämän hakemiston avulla.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Tietokannan luominen **ContosoSQL1** kutsutaan **MyDB1**, varmuuskopiot ja log varmuuskopiointi ja palauttaminen **ContosoSQL2** , jos **WITH NORECOVERY** -vaihtoehto.

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. SQL Server-VMs ryhmän päätepisteet luoda käytettävyyttä ja määritä oikeuksia päätepisteet.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Luo käytettävyys replikoita.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Lopuksi käytettävyys-ryhmän luominen ja liittyä toissijainen se käytettävyys-ryhmään.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Seuraavat vaiheet
Olet nyt on ottanut käyttöön SQL Server aina luomalla käytettävyys-ryhmän Azure-tietokannassa. Määritä käytettävyys ryhmän kuuntelija artikkelissa [määrittäminen ILB-listener aina käyttöön käytettävyys ryhmien Azure-tietokannassa](virtual-machines-windows-classic-ps-sql-int-listener.md).

Tietoja muista käyttämisestä SQL Server Azure-kohdassa [SQL Server-Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).
