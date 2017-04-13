<properties
  pageTitle="Yhdistä mukautetun toimialueen ohjauskoneen pilvipalveluun | Microsoft Azure"
  description="Opettele muodostamaan web/työntekijä roolit mukautetun AD-toimialueen PowerShell ja AD-toimialue-tunniste"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Yhteyden muodostaminen mukautetun ylläpidettävä Azure AD-ohjauskoneen Azure Cloud Services-rooleja

Olemme ensin määrittää Virtual Network (VNet) Azure-tietokannassa. Olemme sitten lisätään VNet Active Directory-toimialueen ohjauskoneen (isännöidään Azure Virtual Machine). Syy seuraavaksi Lisää valmiiksi luotuja VNet aiemmin cloud palvelun roolit ja yhdistää ne myöhemmin toimialueen ohjauskoneen.

Ennen kuin Aloita on muutamia kannattaa pitää mielessä:

1.  Tässä opetusohjelmassa käyttää PowerShell, varmista niin, varmista, että sinulla on asennettu PowerShellin Azure ja valmis. Saat apua PowerShellin Azure määrittäminen Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

2.  AD-toimialueen ohjauskoneen ja Web/Työntekijä roolin esiintymät olisikaan VNet.

Noudata Tässä vaiheittaiset ohjeet ja jos kohtaat ongelmia, Anna palautetta alla. Toisen henkilön saavat takaisin sinulle (Kyllä, emme ALANUOLI).

3. Verkkoon, joka viittaa pilveen palvelun <mark>on oltava</mark> **perinteinen virtual verkkoon**.

## <a name="create-a-virtual-network"></a>Luo virtuaalisia verkko

Voit luoda virtuaalisen verkon Azure Azure perinteinen kautta tai PowerShellin avulla. Tässä opetusohjelmassa Käytämme PowerShell. Azure perinteinen portaalissa Virtual verkon luomisesta on artikkelissa [Luo virtuaalisia verkossa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Luo Virtual Machine

Kun olet suorittanut Virtual verkon määrittäminen, sinun on AD-ohjauskoneen luomiseen. Tässä opetusohjelmassa, on olla määrittäminen AD ohjauskoneen Azure Virtual Machine.

Voit tehdä tämän luominen virtual koneen alla komennoilla PowerShellin kautta:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Siirrä ylemmälle tasolle virtuaalikoneen toimialueen ohjauskoneen
Voit määrittää virtuaalikoneen AD-ohjauskoneen, sinun on AM kirjautuminen ja määritä se.

Kirjautua sisään AM voit hakea RDP-tiedoston PowerShellin kautta, käytä alla olevia komentoja.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Kun olet kirjautuneena AM, seuraamalla vaiheittaiset ohjeet [määrittäminen asiakkaan AD toimialueen ohjauskoneen](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx), AD-ohjauskoneen kuin virtuaalikoneen-määritys.

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Lisää pilvipalvelussa sijaitsevaan Virtual verkkoon

Seuraavaksi on lisättävä cloud palvelun käyttöönoton juuri luomasi VNet. Voit tehdä tämän Muokkaa cloud-palvelun cscfg lisäämällä artikkelin olennaisista Kohdista, että cscfg Visual Studio tai valittua-editorilla.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Seuraavaksi muodosta cloud services-projekti ja ota se käyttöön Azure. Saat apua käyttöönotossa Azure cloud services-paketti, katso, [miten voit luominen ja käyttöönotto pilvipalveluun](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Yhteyden muodostaminen toimialueen web/työntekijä roolit

Kun cloud palvelun projektin on otettu käyttöön Azure-yhdistäminen mukautetun AD-toimialueen käyttäminen AD toimialueen pääte roolin esiintymät. Lisää aiemmin cloud services käyttöönoton AD-toimialue-tunniste ja liity mukautettua toimialuetta, suorita seuraavat komennot powershellissä:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

Ja siinä.

Cloud services pitäisi nyt liittää mukautetun toimialueen ohjauskoneen. Jos haluat lisätietoja AD-toimialueen pääte määrittämisestä käytettävissä olevat vaihtoehdot, käytä PowerShell-Ohje, alla kuvatulla tavalla.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
