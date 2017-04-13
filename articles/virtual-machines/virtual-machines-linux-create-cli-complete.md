
<properties
   pageTitle="Luo valmis käyttämällä Azure-CLI Linux-ympäristön | Microsoft Azure"
   description="Tallennustilan, Linux AM, virtual verkon ja aliverkon, kuormituksen tasauspalvelun, NIC, julkiseen IP ja verkko-käyttöoikeusryhmän, kaikki alkava käyttämällä Azure-CLI luominen."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Valmis Linux-ympäristön luominen Azure-CLI avulla

Tässä artikkelissa on luoda yksinkertaisen verkon kuormituksen tasauspalvelun ja VMs, joka sopii kehittäminen ja yksinkertainen tietojenkäsittely kahdet. Selkeät prosessin mukaan command-komento, kunnes sinulla on kaksi työ, suojattu Linux VMs, joihin voit muodostaa yhteyden mihin tahansa Internetissä. Valitse siirtäminen monimutkaisia verkkoihin ja ympäristöissä.

Muutamiin voit tietoja riippuvuuden hierarkian Resurssienhallinta käyttöönotto-mallin avulla voit, ja siitä, kuinka paljon power se sisältää. Kun näet, kuinka järjestelmä on muodostettu, voit muodostaa tämän paljon nopeammin [Azure Resurssienhallinta mallien](../resource-group-authoring-templates.md)avulla. Myös kun kerrotaan, miten ympäristön osat sopivat yhteen, voit automatisoida ne mallien luominen on helpompaa.

Ympäristön on:

- Kaksi VMs sisällä käytettävyys määrittäminen.
- Kuormituksen tasaamisen säännön porttiin 80 kuormituksen.
- Verkon oman AM suojautua ei-toivottuja liikenne ryhmän (NSG) suojaussäännöt.

![Tavallinen ympäristön yleiskatsaus](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Voit luoda tämän mukautetun ympäristössä, sinun on uusin [Azure CLI](../xplat-cli-install.md) Resurssienhallinta-tilassa (`azure config mode arm`). Sinun on myös JSON, jäsentäminen työkalu. Tässä esimerkissä käytetään [jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Pikakomennot
Jos haluat tehdä nopeasti tehtävän osan seuraavat asiat kantaluku AM lataaminen Azure komennot. Lisää yksityiskohtaiset tiedot ja kontekstia loput asiakirjasta löytyvät jokaisen vaiheen aloitus [tähän](#detailed-walkthrough).

Varmista, että sinulla on [Azure-CLI](../xplat-cli-install.md) kirjautuneena palveluun ja Resurssienhallinta-tilassa:

```bash
azure config mode arm
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ovat `myResourceGroup`, `mystorageaccount`, ja `myVM`.

Luo resurssiryhmän. Seuraavassa esimerkissä luodaan nimeltä resurssiryhmä `myResourceGroup` - `westeurope` sijainti:

```bash
azure group create -n myResourceGroup -l westeurope
```

Tarkista resurssiryhmän JSON-jäsentimen avulla:

```bash
azure group show myResourceGroup --json | jq '.'
```

Tallennustilan tilin luominen Seuraavassa esimerkissä luodaan tallennustilan-tili nimeltä `mystorageaccount` (tallennustilan tilin nimi on oltava yksilöllinen, anna niin oman yksilöllinen nimi):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Tallennustilan-tilin vahvistaminen JSON-jäsentimen avulla:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Luo virtuaalisia verkkoon. Seuraavassa esimerkissä luodaan virtual nimeltä verkon `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Luo aliverkon. Seuraavassa esimerkissä luodaan nimeltä aliverkon `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Tarkista VPN ja aliverkon JSON-jäsentimen avulla:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Luo julkinen IP. Seuraavassa esimerkissä luodaan julkiseen IP, nimeltä `myPublicIP` DNS nimellä `mypublicdns` (DNS-nimen on oltava yksilöllinen, niin säädetään oman yksilöllinen nimi):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Luo kuormituksen. Seuraavassa esimerkissä luodaan kuormituksen, jonka nimi on `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Luo edusta IP-ryhmän kuormituksen, ja liitä julkiseen IP. Seuraavassa esimerkissä luodaan edusta IP-ryhmän nimeltä `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Saat kuormituksen taustatietokantaan IP-ryhmän luominen Seuraavassa esimerkissä luodaan taustatietokantaan IP resurssivarantoon nimeltä `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Luo SSH saapuvien verkon kuormituksen osoite muuntaminen (NAT) säännöt. Seuraavassa esimerkissä luodaan kaksi kuormituksen tasauspalvelun sääntöä `myLoadBalancerRuleSSH1` ja `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Luo sivusto saapuvan liikenteen säännöt NAT kuormituksen. Seuraavassa esimerkissä luodaan kuormituksen tasauspalvelun säännön nimi`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Luo kuormituksen tasauspalvelun kunto näytteenottimen. Seuraavassa esimerkissä luodaan nimeltä TCP-näytteenottimen `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Tarkista kuormituksen, IP jakavat ja NAT säännöt JSON-jäsentimen avulla:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Luo ensimmäinen verkkokortti (NIC). Korvaa `#####-###-###` osia käyttämällä oman Azure tilauksen. Tilauksen tunnus on mainittu tulosteen `jq` tutkittaessa luot resurssit. Voit myös tarkastella tilauksen-tunnus ja `azure account list`. 

Seuraavassa esimerkissä luodaan NIC, jonka nimi on `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Luo toinen NIC-sivustosta. Seuraavassa esimerkissä luodaan NIC, jonka nimi on `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Tarkista kaksi NIC JSON-jäsentimen avulla:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Verkko-käyttöoikeusryhmän luominen Seuraavassa esimerkissä luodaan nimeltä verkon käyttöoikeusryhmän `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Lisää verkko-käyttöoikeusryhmän kaksi saapuvan liikenteen säännöt. Seuraavassa esimerkissä luodaan kaksi sääntöä `myNetworkSecurityGroupRuleSSH` ja `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Tarkista saapuvan liikenteen säännöt ja verkko-käyttöoikeusryhmän JSON-jäsentimen avulla:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Verkko-käyttöoikeusryhmän sitoa kaksi NIC:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Käytettävyys-joukon luominen. Seuraavassa esimerkissä luodaan määrittää nimetyn saatavuus `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Luo ensimmäinen Linux AM. Seuraavassa esimerkissä luodaan AM, jonka nimi on `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Luo toinen Linux AM. Seuraavassa esimerkissä luodaan AM, jonka nimi on `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Käytä JSON-jäsentimen varmistamaan, että kaikki tiedot, jotka on luotu:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Vie uuteen ympäristöön mallia, voit luoda uusia esiintymiä nopeasti uudelleen:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Laskun
Yksityiskohtaiset vaiheet, jotka kerrotaan, mitä jokaisen komennon jokaisen kuin ympäristön muodostetaan. Käsitteistä on hyötyä, kun luot omia mukautettuja ympäristöissä kehitys- tai.

Varmista, että sinulla on [Azure-CLI](../xplat-cli-install.md) kirjautuneena palveluun ja Resurssienhallinta-tilassa:

```bash
azure config mode arm
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ovat `myResourceGroup`, `mystorageaccount`, ja `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Resurssiryhmien luominen ja valitse käyttöönoton sijainnit

Azure resurssiryhmät ovat looginen käyttöönoton kohteet, jotka sisältävät tiedot ja metatiedot käyttöön resurssin ominaisuuksissa looginen hallinta. Seuraavassa esimerkissä luodaan nimeltä resurssiryhmä `myResourceGroup` - `westeurope` sijainti:

```bash
azure group create --name myResourceGroup --location westeurope
```

Tulos:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

Tarvitset tallennustilan tilit AM-levyille ja kaikki tiedot levyjen, johon haluat lisätä. Voit luoda tallennustilan tilien lähes välittömästi, kun resurssiryhmien luominen.

Käytämme tähän `azure storage account create` komennon kulkeva sijainti resurssiryhmä-tilin, joka ohjaa ja tallennustilaa tuki haluamasi tyyppi. Seuraavassa esimerkissä luodaan tallennustilan-tili nimeltä `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Tulos:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Tutkia käyttämällä Microsoftin resurssiryhmä `azure group show` komennon, käytetään [jq](https://stedolan.github.io/jq/) -työkalun mukana `--json` Azure CLI-vaihtoehto. (Voit käyttää **jsawk** tai minkä tahansa kielen kirjaston haluat jäsentää JSON.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Tulos:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Voit tutkia tallennustilan-tilin avulla CLI, sinun on Määritä tilien nimet ja avaimet. Korvaa nimi seuraavassa esimerkissä tallennustilan tilin nimi, joka on valittava:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Sitten voit helposti tarkastella tallennustilan tiedot:

```bash
azure storage container list
```

Tulos:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>VPN- ja aliverkon luominen

Seuraavaksi aiot tarvittavan käynnissä Azure ja aliverkon, jossa voit luoda oman VMs virtual verkkoon. Seuraavassa esimerkissä luodaan virtual nimeltä verkon `myVnet` kanssa `192.168.0.0/16` osoitteen etuliite:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Tulos:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Uudelleen käytetään--json voi `azure group show` ja **jq** nähdäksesi, kuinka Microsoft olet muodostamassa resurssien. Nyt on `storageAccounts` resurssin ja `virtualNetworks` resurssi.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Tulos:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Nyt luominen-aliverkon `myVnet` virtual verkon, johon VMs on otettu käyttöön. Käytämme `azure network vnet subnet create` komennon sekä resursseja, että olet jo luonut: `myResourceGroup` resurssiryhmä ja `myVnet` VPN. Seuraavassa esimerkissä lisäämme nimeltä aliverkon `mySubnet` kanssa aliverkon osoite etuliite `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Tulos:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Koska aliverkon on loogisesti virtual verkoston sisällä, Odotamme aliverkon tiedot hieman eri tavalla-komennolla. Käytämme komento on `azure network vnet show`, mutta on edelleen tutkia JSON tulosteen **jq**avulla.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Tulos:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Luo julkinen IP-osoite (PIP)

Nyt julkinen IP-osoite (PIP), joka on määrittää kuormituksen tasauspalvelun luominen. Voit muodostaa yhteyttä VMs Internetin avulla `azure network public-ip create` komento. Koska oletusarvo-osoite on dynaaminen, on nimetty DNS tekstin luominen **cloudapp.azure.com** toimialueen avulla `--domain-name-label` vaihtoehto. Seuraavassa esimerkissä luodaan julkiseen IP, nimeltä `myPublicIP` DNS nimellä `mypublicdns`. Kun DNS-nimen on oltava yksilöllinen, on yksilöllinen omia DNS-nimi:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Tulos:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Julkinen IP-osoite on myös ylimmän tason resurssi, jotta näet sen `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Tulos:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Lisää resurssitiedot, kuten alitoimialueen, täydellinen toimialuenimi (FQDN) valmis käyttämällä voit tutkia `azure network public-ip show` komento. Julkisen IP-osoite-resurssi on varattu loogisesti, mutta tietyn sähköpostiosoitteen ei vielä ole määritetty. IP-osoitteen hankkiminen aiot on kuormituksen, jossa emme ole vielä luonut.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Tulos:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Kuormituksen tasaus- ja IP-jakavat luominen
Luodessasi kuormituksen tasauspalvelun, voit jakaa useita VMs liikenne. Sovelluksen luotettavuutta sisältää myös useita VMs, joka vastaa käyttäjän ylläpito tai paksu Lataa suorittamalla. Seuraavassa esimerkissä luodaan kuormituksen, jonka nimi on `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Tulos:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Tutustu kuormituksen on melko tyhjä, joten katsotaan seuraavaksi luoda joitakin IP. Haluat luoda kaksi IP, tutustu kuormituksen - yksi edustatietokanta ja yksi uudelleen. Edustatietokannan IP-ryhmän näkyy julkisesti. Kannattaa myös sijainti, johon on määrittäminen PIP, joka on luotu aiemmassa versiossa. Valitse Käytämme taustatietokantaan resurssivarantoon Microsoftin VMs sijainniksi muodostaa yhteyden. Sen mukaan, liikenne voidaan flow kuormituksen, VMs kautta.

Ensin luodaan Microsoftin edusta IP-ryhmän. Seuraavassa esimerkissä luodaan nimeltä edusta resurssivarantoon `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Tulos:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Huomaa, miten on käytetty `--public-ip-name` Siirry välitä `myPublicIP` , joka on luotu aiemmassa versiossa. Julkinen IP-osoite määritteleminen kuormituksen voit saavuttaa oman VMs Internetissä.

Seuraavaksi luodaan Microsoftin toisen IP-ryhmän Microsoftin taustatietokantaan tietoliikenteen tällä hetkellä. Seuraavassa esimerkissä luodaan nimeltä taustatietokantaan resurssivarantoon `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Tulos:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Näemme, miten Microsoftin kuormituksen jokaisen katsomalla kanssa `azure network lb show` ja JSON tulos:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Tulos:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Kuormituksen NAT sääntöjen luominen
Liikenne juoksutus Microsoftin kuormituksen kautta pääset annettava luominen verkko-osoitteen käännöksen (NAT) säännöt, jotka määrittävät saapuvat tai Lähtevät toiminnot. Määritä käytettävä protokolla ja valitse Yhdistä ulkoisten porttien sisäinen portit haluamallasi tavalla. Tutustu ympäristössä luominen joitakin säännöt, jotka sallivat SSH Microsoftin kuormituksen, tutustu VMs kautta. Olemme määrittäminen TCP-portit 4222 ja 4223 ohjaamaan TCP-porttia 22 Microsoftin VMs (joka luodaan myöhemmin). Seuraava esimerkki luo säännön, jonka nimi `myLoadBalancerRuleSSH1` yhdistämään porttinumeroa 4222 porttiin 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Tulos:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Toista nämä vaiheet NAT toisen säännön SSH. Seuraava esimerkki luo säännön, jonka nimi `myLoadBalancerRuleSSH2` yhdistämään porttinumeroa 4223 porttiin 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Oletetaan, että myös Siirry eteenpäin ja web tietoliikenteen liitettäessä säännön ylöspäin Microsoftin IP-jakavat portti TCP 80 NAT säännön. Jos olemme yhdistää IP-ryhmään, sen sijaan, että liitettäessä erikseen, tutustu VMs säännön sääntö on lisätä tai poistaa VMs IP-varannon. Kuormituksen säätää automaattisesti liikenne etenemisen. Seuraava esimerkki luo säännön, jonka nimi `myLoadBalancerRuleWeb` yhdistämään porttinumeroa 80 portti 80:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Tulos:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Kuormituksen tasauspalvelun kunto-näytteenottimen luominen

Ihmisten terveyden tutkia säännöllisesti tarkastukset VMs takana Microsoftin kuormituksen, varmista, että ne toimivat ja vastata määritysten mukaisesti. Jos et, ne poistetaan että varmistaa, että käyttäjät eivät ole ohjataan tietyille käyttäjille. Voit määrittää mukautetun etsii kunto keräysputken, sekä välit ja aikakatkaisun arvoja. Saat lisätietoja kunto keräysputkien [kuormituksen probes](../load-balancer/load-balancer-custom-probe-overview.md). Seuraavassa esimerkissä luodaan TCP kunto koettaa nimetyn `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Tulos:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Tässä on määritetty Microsoftin terveystarkastukset 15 sekunnin välein. Emme voi huomaamatta enintään neljä keräysputkien (minuutin) ennen kuormituksen katsoo isännän ei enää toimi.

## <a name="verify-the-load-balancer"></a>Tarkista kuormituksen
Nyt kuormituksen tasauspalvelun määritykset on tehty. Seuraavassa on vaiheet, joita noudatit:

1. Ensimmäisen kerran kuormituksen.
2. Valitse luotu edusta IP-ryhmän ja määritetty julkiseen IP.
3. Seuraavaksi luomasi taustatietokantaan IP resurssivarantoon, VMs pystyy muodostamaan yhteyden.
4. Tämän jälkeen voit luoda NAT säännöt, jotka sallivat SSH VMs hallinnoinnista sekä sääntö, joka sallii porttinumeroa 80 Microsoftin online.
5. Lopuksi lisäsit kunto näytteenottimen, voit tarkistaa ajoittain VMs. Kunto-näytteenottimen varmistaa, että käyttäjät eivät yrität käyttää AM, joka ei enää toimi tai käsittelevä sisältöä.

Seuraavaksi tarkastellaan kuormituksen tasaus näyttää nyt tältä:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Tulos:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Luo NIC Linux AM käyttäminen

NIC ovat ohjelmallisesti käytettävissä, koska voi käyttää sääntöjä niiden käyttöä. Voit myös on useampi kuin yksi. Seuraavassa `azure network nic create` komennon yhdistää NIC kuormituksen taustatietokantaan IP-ryhmään, ja liittää sen sallimaan SSH tietoliikenne NAT-säännön.
 
Korvaa `#####-###-###` osia käyttämällä oman Azure tilauksen. Tilauksen tunnus on mainittu tulosteen `jq` tutkittaessa luot resurssit. Voit myös tarkastella tilauksen-tunnus ja `azure account list`. 

Seuraavassa esimerkissä luodaan NIC, jonka nimi on `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Tulos:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Näet tietoja tarkastelemalla resurssia suoraan. Resurssin tutkia avulla `azure network nic show` komento:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Tulos:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Nyt luoda toisen NIC-liitettäessä Microsoftin taustatietokantaan IP-ryhmään uudelleen. Toinen aika NAT säännöllä sallii SSH liikenne. Seuraavassa esimerkissä luodaan NIC, jonka nimi on `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Verkon käyttöoikeusryhmän ja sääntöjen luominen

Nyt luoda verkon käyttöoikeusryhmän ja saapuvan liikenteen säännöt, jotka ohjaavat access NIC-sivustosta. Verkon käyttöoikeusryhmän voi suojata NIC tai aliverkon. Voit määrittää ohjaamaan ja siitä uloskirjautuminen oman VMs liikenteen säännöt. Seuraavassa esimerkissä luodaan nimeltä verkon käyttöoikeusryhmän `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Lisääminen saapuvien säännön NSG sallimaan saapuvia yhteyksiä porttiin 22 (tukemaan SSH). Seuraava esimerkki luo säännön, jonka nimi `myNetworkSecurityGroupRuleSSH` sallimaan porttinumeroa 22:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nyt lisääminen saapuvien säännön NSG sallimaan saapuvia yhteyksiä porttiin 80 (tukemaan web liikenne). Seuraava esimerkki luo säännön, jonka nimi `myNetworkSecurityGroupRuleHTTP` sallimaan porttinumeroa 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] Saapuvan liikenteen sääntö on saapuvien verkkoyhteyksien suodatin. Tässä esimerkissä on sidottava NSG VMs virtual Verkkokortti, mikä tarkoittaa, että pyyntö porttiin 22 välitetään kautta Verkkokortti, tutustu AM. Verkkoyhteyden käsittelee saapuvia säännöllä ja ei tietoja päätepisteen, joka on mitä olisi tietoja perinteinen käyttöönotoissa. Voit avata portin, jätä `--source-port-range` arvoksi "\*" (oletusarvo) Hyväksy saapuva pyynnöt **minkä tahansa** pyytää portin. Portit ovat yleensä dynaamisia.

## <a name="bind-to-the-nic"></a>Verkkokortti sitoa

Sido NIC NSG. Tutustu NIC yhteyttä Microsoftin verkko-käyttöoikeusryhmän annettava. Suorita molemmat komennot, voit yhdistää molempien Microsoftin NIC:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Käytettävyys-joukon luominen
Käytettävyys määrittää ohjeen levitä oman VMs vika toimialueet ja Päivitä toimialueet. Luo määrittäminen oman VMs saatavuus. Seuraavassa esimerkissä luodaan määrittää nimetyn saatavuus `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Vian toimialueiden määrittäminen ryhmittely näennäiskoneiden, joilla on yhteiset power lähde- ja vaihda. Oletusarvon mukaan, jotka on määritetty käytettävyys asettaa näennäiskoneiden on erotettu enintään kolme vika toimialueilla. On laitteisto-ongelma johonkin näistä vika toimialueiden ei vaikuta jokaisen AM, jossa sovellus on käynnissä. Azure jakaa automaattisesti VMs vika toimialueet puhelua ne käytettävyys määrittäminen.

Päivityksen toimialueiden osoittavat näennäiskoneiden ja pohjana fyysinen laite, voit käynnistettävä samanaikaisesti. Siinä järjestyksessä, jossa päivityksen toimialueet on käynnistettävä eivät välttämättä ole peräkkäisiä suunnitellun ylläpidon aikana, mutta vain yhden päivityksen käynnistetään kerrallaan. Uudelleen Azure automaattisesti jakaa oman VMs päivityksen toimialueilla puhelua ne käytettävyys-sivustossa.

Lisätietoja [VMs käytettävyyden hallinta](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>Luo Linux VMs

Olet luonut tukemaan Internet niitä VMs säilytys- ja resursseja. Nyt luodaan ne VMs ja suojaaminen SSH-näppäintä, joka ei ole salasanan. Tässä tapauksessa tutustutaan luominen Ubuntu AM perusteella viimeisimmän l.. Olemme Etsi kuva tiedot käyttämällä `azure vm image list`, kuvatulla tavalla [Azure AM kuvien etsimiseen](virtual-machines-linux-cli-ps-findimage.md).

On valittu kuva-komennon avulla `azure vm image list westeurope canonical | grep LTS`. Tässä tapauksessa Käytämme `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Viimeisessä kentässä, emme välittää `latest` niin, että tulevaisuudessa on aina siirtyä viimeisimmän muodosta. (Merkkijono, Käytämme on `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Seuraavaksi tämän tuttuja kaikki, jotka on jo luotu ssh rsa julkisen ja yksityisen avaimen pariliitos Linux-tai Mac-tietokoneeseen käyttämällä **ssh-keygen-t rsa -b 2048**. Jos sinulla ei ole mitään varmenteen avaimen paria että `~/.ssh` kansioon, voit luoda ne:

- Automaattisesti, käyttämällä `azure vm create --generate-ssh-keys` vaihtoehto.
- Manuaalisesti käyttämällä [Voit luoda itse ohjeita](virtual-machines-linux-mac-create-ssh-keys.md).

Vaihtoehtoisesti voit käyttää `--admin-password` menetelmä tarkistamiseen SSH yhteydet AM luomisen jälkeen. Tämä menetelmä ei yleensä ole yhtä turvallinen.

Tuomalla sekä resursseja ja tietoja yhdessä luodaan AM `azure vm create` komento:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Tulos:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Voit muodostaa yhteyden oman AM heti oletusarvon SSH-näppäimiä käyttämällä. Varmista, että määrität oikea portti, koska emme ole kulkee kuormituksen kautta. (Microsoftin ensimmäisen AM olemme Määritä NAT-säännön välittäminen portin 4222 sekä AM):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Tulos:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Siirry eteenpäin ja luo toinen AM samalla tavalla:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Voit nyt käyttää ja `azure vm show myResourceGroup myVM1` komento tarkistaa, mitä olet luonut. Tässä vaiheessa käytössäsi on Ubuntu-VMs takana kuormituksen Azure-tietokannassa, kirjaudu sisään vain SSH-avaimen pair (koska salasanat on poistettu käytöstä). Voit asentaa nginx tai httpd, web-sovelluksen käyttöönotto ja nähdä liikenne sekä VMs kuormituksen kulku.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Tulos:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Vie ympäristön tallentaminen mallina
Nyt kun olet luonut ulos tässä ympäristössä, jos haluat luoda lisää kehitysympäristö samat parametrit tai tuotantoympäristössä, joka vastaa sitä? Resurssienhallinta käyttää JSON-mallit, jotka määrittävät kaikki parametrit-ympäristössä. Koko ympäristöissä muodostetaan viittaamalla JSON-malli. Voit [luoda JSON mallit manuaalisesti](../resource-group-authoring-templates.md) tai aiemmin ympäristössä luovan JSON-mallin vieminen:

```bash
azure group export --name myResourceGroup
```

Tämä komento luo `myResourceGroup.json` tiedoston nykyisen toimimasta hakemistossa. Kun luot ympäristössä tätä mallia, sinua kehotetaan kaikkien resurssien nimet, mukaan lukien kuormituksen, verkkoliittymät tai VMs nimet. Voit täyttää seuraavat nimet lisäämällä mallitiedosto `-p` tai `--includeParameterDefaultValue` parametri `azure group export` komento, joka näkyy aiemmin. Voit määrittää resurssien nimet tai [Luo parameters.json tiedosto](../resource-group-authoring-templates.md#parameters) , joka määrittää resurssinimien JSON mallin muokkaaminen.

Voit luoda ympäristössä mallin seuraavasti:

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Voit lukea [Lisätietoja käyttöönottamisesta malleista](../resource-group-template-deploy-cli.md). Lue lisätietoja vaiheittainen Päivitä ympäristöissä, Määritä parametrit-tiedosto ja käyttää malleja yksittäisen tallennuspaikka.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt olet valmis aloittamaan useita verkko-osien ja VMs käyttäminen. Voit malli-ympäristöä muodostetaan sovelluksen käyttämällä core-komponentit käyttöön tässä.
