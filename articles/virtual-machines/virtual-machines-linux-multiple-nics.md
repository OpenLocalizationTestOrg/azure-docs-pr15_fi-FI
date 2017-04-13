<properties
   pageTitle="Luo Linux AM useita NIC | Microsoft Azure"
   description="Opi luomaan Linux AM sisältää useita NIC liitetään Azure CLI tai Resurssienhallinta-mallien avulla."
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
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Luomalla Linux AM useita NIC
Voit luoda virtual machine (AM) Azure-tietokannassa, jossa on useita VPN-liittymät (NIC) liitetään. Käytetty vaihtoehto olisi on eri aliverkosta edusta- ja yhteyden tai verkon omistautunut seurantaa tai varmuuskopio-ratkaisun. Tässä artikkelissa on luomalla AM liitetty useita NIC Pikakomennot. Lisätietoja on myös useita NIC sisällä Bash omia komentosarjoja luomisesta lukea lisää tietoja [usean NIC VMs käyttöönotto](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Eri [AM koot](virtual-machines-linux-sizes.md) Tukipuhelinnumero erilaisten NIC-kokoa niin että AM vastaavasti.

>[AZURE.WARNING] Sinun on liitettävä useita NIC, kun luot AM - NIC ei voi lisätä aiemmin AM. Voit [luoda AM, joka perustuu alkuperäiseen virtual näiden levyjen asemasta](virtual-machines-linux-copy-vm.md) ja luoda useita NIC, kun otat käyttöön AM.

## <a name="quick-commands"></a>Pikakomennot
Varmista, että sinulla on [Azure CLI](../xplat-cli-install.md) kirjautuneena palveluun ja Resurssienhallinta-tilassa:

```bash
azure config mode arm
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ohjelmistopakettia `myResourceGroup`, `mystorageaccount`, ja `myVM`.

Luo resurssiryhmä. Seuraavassa esimerkissä luodaan nimeltä resurssiryhmä `myResourceGroup` - `WestUS` sijainti:

```bash
azure group create myResourceGroup -l WestUS
```

Tallennustilan tilin pitoon oman VMs luominen. Seuraavassa esimerkissä luodaan tallennustilan-tili nimeltä `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Luo virtuaalisia verkko muodostaa yhteyttä VMs. Seuraavassa esimerkissä luodaan virtual nimeltä verkon `myVnet` osoitteen etuliite jossa `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Luo kaksi VPN - aliverkosta yksi edusta liikenteen ja yksi taustatietokantaan tietoliikenteen. Seuraavassa esimerkissä luodaan kahden aliverkon-niminen `mySubnetFrontEnd` ja `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Luo kaksi NIC-yksi Verkkokortti edusta aliverkon, ja yhden NIC liittäminen taustatietokantaan aliverkon. Seuraavassa esimerkissä luodaan kahden NIC-niminen `myNic1` ja `myNic2`, ja liittää ne oman aliverkosta:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Lopuksi voit luoda oman AM liittämisen aiemmin luotu kaksi NIC. Seuraavassa esimerkissä luodaan AM, jonka nimi on `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Useita NIC käyttämällä Azure CLI luominen
Jos olet aiemmin luonut AM, Azure-CLI avulla, Pikakomennot pitäisi olla tuttuja. Prosessi on sama NIC yksi tai useita NIC luomiseen. Lue lisätietoja [käyttöönotto useita NIC Azure-CLI käyttäminen](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), mukaan lukien komentosarjat prosessi, jossa toistoasetuksia avulla voit luoda kaikki NIC.

Seuraavassa esimerkissä luodaan kahden NIC-niminen `myNic1` ja `myNic2`, jossa yksi Verkkokortti kunkin aliverkon yhteyden:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Yleensä voit myös luoda [Verkko-käyttöoikeusryhmän](../virtual-network/virtual-networks-nsg.md) tai [kuormituksen](../load-balancer/load-balancer-overview.md) avulla hallita ja jakaa oman VMs liikenne. Uudelleen-komennot ovat samat käsittelyyn liittyvistä useita NIC. Seuraavassa esimerkissä luodaan nimeltä verkon käyttöoikeusryhmän `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Verkko-käyttöoikeusryhmän käyttämällä omaa NIC sitoa `azure network nic set`. Seuraavassa esimerkissä sitoo `myNic1` ja `myNic2` kanssa `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Luodessasi AM voit määrittää nyt useita NIC. Sen sijaan, että käyttämällä `--nic-name` antamaan yhden NIC sen sijaan voit käyttää `--nic-names` ja anna NIC CSV-luettelo. Sinun on myös huolehtia, kun valitset AM kokoa. Rajoitukset, voit lisätä AM NIC kokonaismäärän on. Lisätietoja [Linux AM koot](virtual-machines-linux-sizes.md). Seuraavassa esimerkissä esitetään, miten voit määrittää useita NIC ja AM koko, joka tukee useita NIC (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Luominen useita NIC Resurssienhallinta mallien käyttäminen
Azure Resurssienhallinta malleja määritettäviä JSON-tiedostojen avulla voit määrittää ympäristösi. Voit lukea [Azure resurssien hallinnan yleiskatsaus](../azure-resource-manager/resource-group-overview.md). Resurssienhallinta mallit avulla voit luoda useita kertoja resurssin käyttöönoton, kuten luominen useita NIC aikana. *Kopioi* avulla voit määrittää minuuttimäärän, jonka esiintymät luominen:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Lisätietoja [luominen *kopioimalla*useita kertoja](../resource-group-create-multiple.md). 

Voit myös käyttää `copyIndex()` liittäminen luvun sitten resurssinimi, jonka avulla voit luoda `myNic1`, `myNic2`jne. Seuraavassa kuvassa näkyy esimerkki liität indeksiarvo:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Voit lukea täydellinen esimerkki [luominen useita NIC Resurssienhallinta mallien avulla](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Seuraavat vaiheet
Varmista, että voit tarkastella [Linux AM koot](virtual-machines-linux-sizes.md) yritettäessä luomalla AM useita NIC. Kiinnitä huomiota NIC jokaisen AM koon tukee enimmäismäärä. 

Muista, että et voi lisätä uusia NIC aiemmin AM, sinun on luotava kaikki NIC, kun otat käyttöön AM. Huolehtia suunnittelun varmistaaksesi, että kaikki tarvittavat verkkoyhteys sen-käyttöönoton yhteydessä.