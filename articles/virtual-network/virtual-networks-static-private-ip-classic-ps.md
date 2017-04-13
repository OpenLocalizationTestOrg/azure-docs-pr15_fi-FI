<properties 
   pageTitle="Voit määrittää staattinen yksityinen IP perinteistä PowerShellin avulla | Microsoft Azure"
   description="Tietoja staattinen yksityinen IP-osoitteet (tapahtuneen) ja kuinka voit hallita niiden perinteinen tila ja PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Staattinen yksityisen IP-osoitteen määrittämisestä (perinteinen) powershellissä

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [hallita staattinen yksityinen IP-osoite-resurssien hallinnan käyttöönottomalli](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Alla olevassa otoksen PowerShell-komennoilla toiminta yksinkertainen ympäristössä, jossa on jo luotu. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa, luo ensin [Create VNet](virtual-networks-create-vnet-classic-netcfg-ps.md)kuvattu testiympäristössä.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>IP-osoite on käytettävissä tarkistaminen
Voit tarkistaa, onko IP-osoite *192.168.1.101* käytettävissä VNet, jonka nimi on *TestVNet*, suorita seuraava komento PowerShell ja Vahvista arvon *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Odotettu tulos:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Määrittäminen staattinen yksityinen IP-osoite AM luotaessa
PowerShell-komentosarjaa Luo uusi pilvipalvelussa nimeltä *TestService*sitten kuvan hakee Azure, Luo AM, nimeltä *DNS01* käyttämällä haetut kuvaa uuden pilvipalvelussa, asettaa *edusta*-niminen aliverkon olevan AM ja määrittää *192.168.1.7* kuin staattinen yksityinen IP-osoite AM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Odotettu tulos:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Voit noutaa staattisen yksityinen IP-osoitetiedot AM varten
Staattinen yksityinen IP-osoitetiedot, joka on luotu käyttämällä yllä komentosarja AM katselemista varten suorittamalla seuraavan PowerShell-komennon ja noudata arvot, *IP-osoite*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Odotettu tulos:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Staattinen yksityinen IP-osoite poistamisesta AM
Voit poistaa staattinen yksityinen IP-osoite lisätään AM edellä komentosarjan suorittaa seuraavan PowerShell-komennon:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Odotettu tulos:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Staattinen yksityisen IP-osoitteen lisääminen aiemmin luotuun AM
Voit lisätä staattisen yksityinen IP-osoite, luotu edellä runt komentosarja AM seuraavan komennon:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Odotettu tulos:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [varattu julkiseen IP](virtual-networks-reserved-public-ip.md) -osoitteet.
- Lisätietoja [esiintymän tason julkiseen IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -osoitteista.
- Tietojen kysyminen [varattu IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).
