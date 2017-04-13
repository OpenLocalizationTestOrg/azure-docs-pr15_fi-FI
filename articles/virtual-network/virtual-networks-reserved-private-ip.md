<properties 
   pageTitle="Staattinen Sisäinen yksityinen IP määrittäminen"
   description="Tietoja staattinen sisäinen IP-osoitteet (tapahtuneen) ja miten niiden hallintaa varten"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Staattinen sisäinen yksityisen IP-osoitteen PowerShellin (perinteinen) määrittäminen
Useimmissa tapauksissa sinun ei tarvitse määrittää virtuaalikoneen staattinen sisäinen IP-osoite. VMs virtual verkossa tulee automaattisesti sisäinen IP-osoite alueelta, jolla voit määrittää. Mutta joissakin tapauksissa määrittäminen tietyn AM staattinen IP-osoite on järkevää. Jos esimerkiksi oman AM kertoo, jos haluat suorittaa DNS tai päivitetään toimialueen ohjauskoneen. Staattinen sisäinen IP-osoite säilyy AM jopa – Pysäytä/deprovision tilan kanssa. 

> [AZURE.IMPORTANT] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Resurssienhallinta ja perinteinen](../resource-manager-deployment-model.md). Tässä artikkelissa käsitellään perinteinen käyttöönotto-mallin. Microsoft suosittelee, että useimmat uudet käyttöönoton käyttävät [resurssien hallinnan käyttöönottomalli](virtual-networks-static-private-ip-arm-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>IP-osoite on käytettävissä tarkistaminen
Voit tarkistaa, onko IP-osoite *10.0.0.7* käytettävissä vnet, jonka nimi on *TestVnet*, suorita seuraava komento PowerShell ja Vahvista arvon *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Jos haluat esikatsella komento edellä turvallisten ympäristön seuraa Profiilikuva [Luo virtuaalisia verkon](virtual-networks-create-vnet-classic-portal.md) luomiseen vnet *TestVnet* nimi ja varmista, että käytössä *10.0.0.0/8* -osoitetilaa.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Staattinen sisäinen IP määrittämiseen AM luominen
PowerShell-komentosarjaa Luo uusi pilvipalvelussa nimeltä *TestService*, sitten kuvan hakee Azure-ja valitse Luo AM, nimeltä *TestVM* käyttämällä haetut näköistiedostoa uusi pilvipalvelussa, määrittää aliverkon nimeltä *aliverkon 1*olevan AM ja määrittää *10.0.0.7* kuin staattinen sisäinen IP AM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Voit noutaa staattisen AM sisäinen IP-tiedot
Voit tarkastella AM, joka on luotu käyttämällä yllä komentosarja staattinen IP-sisäinen tietoja suorittaa seuraavan PowerShell-komennon ja noudata arvot, *IP-osoite*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Staattinen sisäinen IP poistamisesta AM
Voit poistaa yllä komentosarjan AM lisätään staattinen sisäinen IP suorittamalla seuraavan PowerShell-komennon:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Staattinen sisäinen IP lisääminen aiemmin luotuun AM
Voit lisätä staattisen sisäinen IP AM luotu käyttäen edellä runt komentosarja seuraavan komennon:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Seuraavat vaiheet

[Varattu IP](virtual-networks-reserved-public-ip.md)

[Esiintymän tason julkiseen IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Varattu IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
