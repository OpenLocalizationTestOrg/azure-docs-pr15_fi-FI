<properties
   pageTitle="Avaa portit ja Linux AM päätepisteet | Microsoft Azure"
   description="Lue, miten voit avata portin / päätepisteen oman Linux AM Azure resurssien hallinnan käyttöönotto-malli ja Azure-CLI luominen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Portit ja Linux AM päätepisteet avaaminen Azure-tietokannassa
Avaa portti tai verkon suodattimen aliverkon tai AM verkkoliittymän Azure päätepisteen virtual tietokoneeseen (AM) luomalla. Suodattimia, jotka ohjaavat sekä saapuva ja lähtevä liikenne sijoittaa verkon käyttöoikeusryhmän resurssi, joka vastaanottaa liikenteen liitetään. Porttiin 80 käytetään yleisiä Esimerkki web-liikenne.

## <a name="quick-commands"></a>Pikakomennot
Verkon käyttöoikeusryhmän ja sinun on asennettu ja Resurssienhallinta syöttötilan [Azure-CLI](../xplat-cli-install.md) sääntöjen luominen

```bash
azure config mode arm
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ohjelmistopakettia `myResourceGroup`, `myNetworkSecurityGroup`, ja `myVnet`.

Luoda oman verkko-käyttöoikeusryhmän, kirjoittamalla oman nimet ja sijainnin asianmukaisesti. Seuraavassa esimerkissä luodaan nimeltä verkon käyttöoikeusryhmän `myNetworkSecurityGroup` - `WestUS` sijainti:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Lisää sääntö, joka Salli oman webserver HTTP-liikenne (tai valitse vaihtoehto, kuten SSH Accessin tai tietokannan yhteys asetukset). Seuraava esimerkki luo säännön, jonka nimi `myNetworkSecurityGroupRule` sallimaan TCP-liikenne porttiin 80:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Yhdistää verkko-käyttöoikeusryhmän oman AM verkkoliittymän (NIC). Seuraavassa esimerkissä liittää aiemmin luotuun NIC nimeltä `myNic` kanssa nimeltä verkon-käyttöoikeusryhmän `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Voit myös yhdistää verkko-käyttöoikeusryhmän kanssa VPN-aliverkon sijaan vain yhden AM verkkoliittymän. Seuraavassa esimerkissä liittää aiemmin luotuun aliverkon nimeltä `mySubnet` - `myVnet` nimeltä verkon-käyttöoikeusryhmän virtual verkostoa `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Saat lisätietoja verkon käyttöoikeusryhmät
Tässä Pikakomennot mahdollistavat pääset hyvin alkuun liikenne juoksutus oman AM. Verkon käyttöoikeusryhmät on useita ominaisuuksia ja rakeisuuden resurssien käyttöoikeuksien hallintaan. Voit lukea lisää [verkon käyttöoikeusryhmän ja tähän Käyttöoikeusluettelon sääntöjen](../virtual-network/virtual-networks-create-nsg-arm-cli.md)luomisesta.

Voit määrittää verkoston käyttöoikeusryhmiä ja Käyttöoikeusluettelon säännöt Azure Resurssienhallinta mallit osana. Lue lisää [Verkko-käyttöoikeusryhmät mallien](../virtual-network/virtual-networks-create-nsg-arm-template.md)luomisesta.

Jos haluat käyttää portin edelleenlähetystä, voit yhdistää ulkoisen portin oman AM sisäinen porttiin, käytä kuormituksen tasauspalvelun ja verkko-osoitteen käännöksen (NAT) sääntöjä. Haluat ehkä näyttää porttinumeroa 8080 ulkoisesti ja on liikenne ohjataan AM portti TCP 80. Voit opetella [luominen Internetiin yhteydessä oleva-kuormituksen](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä esimerkissä luoda yksinkertaisen säännön sallimaan HTTP-liikenne. Voit etsiä lisätietoja yksityiskohtaisempia ympäristöissä luomisesta on seuraavissa artikkeleissa:

- [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md)
- [Mikä verkon suojauksen ryhmän (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Yleistä kuormituksen tasoitusmääritykset Azure resurssin hallinnasta](../load-balancer2    /load-balancer-arm.md)