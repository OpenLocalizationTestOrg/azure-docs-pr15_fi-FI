<properties 
   pageTitle="Voit siirtää AM tai rooli esiintymän eri aliverkon"
   description="Voit siirtää VMs ja roolin esiintymiä eri aliverkon"
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

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Voit siirtää AM tai rooli esiintymän eri aliverkon

PowerShellin avulla voit siirtää oman VMs yhden aliverkon toiseen samassa virtual verkostossa (VNet). Muokkaaminen CSCFG sijaan PowerShellin avulla voidaan siirtää roolin esiintymät.

>[AZURE.NOTE] Tässä artikkelissa on tietoja, jotka ovat vain Azure perinteinen ominaisuuksissa suhteessa.

Miksi siirtää toiseen aliverkon VMs? Aliverkon siirto on hyödyllinen, kun vanhoja aliverkon on liian pientä ja ei voi laajentaa olemassa olevan käynnissä VMs-kyseisen aliverkon vuoksi. Siinä tapauksessa voit luoda uuden, suurempi aliverkon ja siirtää VMs uusi aliverkon ja kun siirto on valmis, voit poistaa vanhan tyhjä aliverkon.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Voit siirtää toiseen aliverkon AM

Jos haluat siirtää AM, suorita määrittäminen AzureSubnet PowerShell cmdlet-komennon, käyttäen alla olevassa esimerkissä mallina. Seuraavassa esimerkissä on siirtyy aliverkon 2 TestVM sen esitä aliverkosta. Varmista Muokkaa vastaamaan ympäristön esimerkki. Huomaa, että aina, kun suoritat päivityksen AzureVM cmdlet-komento osana ohjeen, se käynnistetään uudelleen oman AM päivityksen yhteydessä.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Jos olet määrittänyt, että AM staattinen sisäisen yksityinen IP, voit on Poista tätä asetusta, ennen kuin voit siirtää AM uusi aliverkon. Tässä tapauksessa toimimalla seuraavasti:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Voit siirtyä toiseen aliverkon rooli-esiintymä

Siirry roolin esiintymän Muokkaa CSCFG-tiedostoa. Seuraavassa esimerkissä on siirtyy "Role0" virtual verkossa *VNETName* sen esitä aliverkosta *aliverkon 2*. Koska roolin esiintymän oli jo otettu käyttöön, vain muutat aliverkon nimi = aliverkon 2. Varmista Muokkaa vastaamaan ympäristön esimerkki.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
