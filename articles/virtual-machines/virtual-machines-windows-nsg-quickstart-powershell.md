<properties
   pageTitle="Avaa portit AM, PowerShellin avulla | Microsoft Azure"
   description="Lue, miten voit avata portin / päätepisteen Azure resurssien hallinnan käyttöönotto tila ja PowerShellin Azure Windows oman AM luominen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Avaaminen portit ja päätepisteet AM Azure PowerShellin avulla
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Pikakomennot
Verkko-käyttöoikeusryhmän ja Käyttöoikeusluettelon sääntöjen luominen on [asennettu PowerShellin Azure uusimman version](../powershell-install-configure.md). Voit myös [Azure-portaalissa seuraavasti](virtual-machines-windows-nsg-quickstart-portal.md).

Kirjautuminen Azure-tili:

```powershell
Login-AzureRmAccount
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ohjelmistopakettia `myResourceGroup`, `myNetworkSecurityGroup`, ja `myVnet`.

Luo sääntö. Seuraava esimerkki luo säännön, jonka nimi `myNetworkSecurityGroupRule` sallimaan TCP-liikenne porttiin 80:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Seuraavaksi verkon käyttäjäryhmien luominen ja määritä HTTP juuri luomasi säännön seuraavasti. Seuraavassa esimerkissä luodaan nimeltä verkon käyttöoikeusryhmän `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Nyt määrittää japanin verkko-käyttöoikeusryhmän aliverkon. Seuraava esimerkki määrittää olemassa olevan virtual verkoston nimeltä `myVnet` muuttujan `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Yhdistää verkko-käyttöoikeusryhmän aliverkon ulkopuolella. Seuraavassa esimerkissä liittää nimeltä aliverkon `mySubnet` verkko-käyttöoikeusryhmän kanssa:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Päivitä lopuksi virtual verkossa, jotta muutokset tulevat voimaan:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Saat lisätietoja verkon käyttöoikeusryhmät
Tässä Pikakomennot mahdollistavat pääset hyvin alkuun liikenne juoksutus oman AM. Verkon käyttöoikeusryhmät on useita ominaisuuksia ja rakeisuuden resurssien käyttöoikeuksien hallintaan. Voit lukea lisää [verkon käyttöoikeusryhmän ja tähän Käyttöoikeusluettelon sääntöjen](../virtual-network/virtual-networks-create-nsg-arm-ps.md)luomisesta.

Voit määrittää verkoston käyttöoikeusryhmiä ja Käyttöoikeusluettelon säännöt Azure Resurssienhallinta mallit osana. Lue lisää [Verkko-käyttöoikeusryhmät mallien](../virtual-network/virtual-networks-create-nsg-arm-template.md)luomisesta.

Jos haluat käyttää portin edelleenlähetystä, voit yhdistää ulkoisen portin oman AM sisäinen porttiin, käytä kuormituksen tasauspalvelun ja verkko-osoitteen käännöksen (NAT) sääntöjä. Haluat ehkä näyttää porttinumeroa 8080 ulkoisesti ja on liikenne ohjataan AM portti TCP 80. Voit opetella [luominen Internetiin yhteydessä oleva-kuormituksen](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä esimerkissä luoda yksinkertaisen säännön sallimaan HTTP-liikenne. Voit etsiä lisätietoja yksityiskohtaisempia ympäristöissä luomisesta on seuraavissa artikkeleissa:

- [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md)
- [Mikä verkon suojauksen ryhmän (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Yleistä kuormituksen tasoitusmääritykset Azure resurssin hallinnasta](../load-balancer/load-balancer-arm.md)