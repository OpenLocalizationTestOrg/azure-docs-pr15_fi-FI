<properties 
   pageTitle="Esiintymän tason julkiseen IP (ILPIP) | Microsoft Azure"
   description="Tietoja ILPIP (PIP) ja miten niitä hallitaan"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Esiintymän tason julkiseen IP yleiskatsaus
Erillisen tason julkiseen IP (ILPIP) on julkinen IP-osoite, voit määrittää suoraan AM tai rooli esiintymän asemesta pilvipalvelussa, jotka sijaitsevat AM tai rooli esiintymä. Tämä ei tapahdu, joka on määritetty pilvipalvelussa sijaitsevaan VIP (virtual IP). Sen sijaan on muita IP-osoite, jonka avulla voit muodostaa AM tai rooli esiintymän.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](virtual-network-ip-addresses-overview-arm.md). 

Varmista, että tiedät, miten [IP-osoitteiden](virtual-network-ip-addresses-overview-classic.md) toimii Azure.

>[AZURE.NOTE] Aiemmin ILPIP on kutsutaan PIP, joka tarkoittaa julkiseen IP-osoite. 

![ILPIP ja VIP välinen ero](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Kuten kuvassa 1, pilvipalvelussa käytetään VIP, käyttämistä, kun yksittäisiä VMs käytetään yleensä käyttämällä VIP:&lt;porttinumero&gt;. Määrittämällä ILPIP tietyn AM, AM voi käyttää suoraan kyseisen IP-osoite.

Kun luot pilvipalveluun Azure-vastaavat DNS A-tietueet luodaan automaattisesti annettavien palvelun kautta täydellinen toimialuenimi (FQDN), todellinen VIP sijaan. Samoja ohjeita tapahtuu ILPIP, sallitaan AM tai rooli esiintymän FQDN sen sijaan, että ILPIP mukaan. Esimerkiksi jos luot nimeltä *contosoadservice*pilvipalveluun ja määrität *contosoweb* ja kaksi esiintymää-roolia, Azure Rekisteröi seuraavat esiintymät A-tietueet:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Voit määrittää vain yhden ILPIP AM tai rooli jokaiselle esiintymälle. Voit käyttää enintään 5 ILPIP päivän tilauskohtaisten. Tällä hetkellä ILPIP ei tueta usean NIC VMs.

## <a name="why-should-i-request-an-ilpip"></a>Miksi ILPIP olisi pyytää?
Jos haluat muodostaa yhteyden AM tai rooli esiintymän suoraan määritetty IP-osoitetta, sijaan pilveen palvelun VIP:&lt;porttinumero&gt;-pyyntö ILPIP oman AM tai rooli-esiintymä.
- **Passiivinen FTP** - mukaan ottaa yhteyttä AM ILPIP voit vastaanottaa liikenne lähes mistä tahansa porttiin, et voi Avaa päätepisteen vastaanottamaan tietoliikennettä. Ottaa käyttöön skenaarioiden, kuten passiivinen FTP, joista portit valitaan dynaamisesti.
- Lähteenä ILPIP ja siirtyy **Lähtevät IP** - liikenteen peräisin AM ja tämä yksilöi ulkoisten kohteiden AM.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Miten pyytää ILPIP AM luonnin aikana
PowerShell-komentosarjaa Luo uusi pilvipalvelussa nimeltä *FTPService*sitten kuvan hakee Azure- ja luo AM, nimeltä *FTPInstance* käyttämällä haetut näköistiedostoa, määrittää käyttämään ILPIP AM ja Lisää uusi palvelu AM:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Voit hakea ILPIP tietoja AM
Saat, joka on luotu käyttämällä yllä komentosarja AM ILPIP suorittaa seuraavan PowerShell-komennon ja noudata *PublicIPAddress* ja *PublicIPName*arvot:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Miten voit poistaa ILPIP AM
Jos haluat poistaa komentosarjan yllä AM lisätään ILPIP, suorita PowerShell-komentoa:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Voit lisätä ILPIP aiemmin AM
Jos haluat lisätä ILPIP luotu edellä komentosarja AM, suorita seuraava komento:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Voit liittää ILPIP, AM palvelun määritys-tiedoston avulla
Voit myös liittää ILPIP, AM palvelun määritys (CSCFG)-tiedoston avulla. Esimerkki xml-alla näkyy pilvipalvelussa käyttämään ILPIP nimetyt *MyPublicIP* rooli-esiintymän määrittäminen: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja [IP-osoitteiden](virtual-network-ip-addresses-overview-classic.md) toiminta perinteinen käyttöönotto-mallissa.

- Lisätietoja [varattu IP-osoitteet](virtual-networks-reserved-public-ip.md).
 