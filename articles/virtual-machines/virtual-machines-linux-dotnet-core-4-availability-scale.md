<properties
   pageTitle="Käytettävyys ja Azure Resurssienhallinta-mallit-asteikkoa | Microsoft Azure"
   description="Azure virtuaalikoneen DotNet Core-opetusohjelma"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Käytettävyys ja Azure Resurssienhallinta mallit-asteikkoa

Käytettävyys ja skaalausta lisätietoja käytettävyyttä ja mahdollisuus täyttävät tarvittaessa. Jos sovellus täytyy olla 99,9 % ajan, se täytyy olla arkkitehtuuri, joka sallii useita samanaikaisia Laske resursseja. Sen sijaan, että yksittäisen sivuston ylemmälle tasolle käytettävyydestä kokoonpanon sisältää esimerkiksi samassa paikassa, ja tasaamisen tekniikka eteen ne useita kertoja. Tässä määrityksessä esiintymän sovellus voidaan ottaa ylläpito, kun jäljellä olevat edelleen. Skaalaa viittaa toisaalta sovellusten mahdollisuus yhteyshenkilönä tarvittaessa. Kuormituksen kanssa soveltaa, lisäämällä tai poistamalla esiintymät olevilla sallii sovelluksen skaalattavissa tarvittaessa.

Tämän asiakirjan tiedot musiikkia kaupan otoksen käyttöönoton määrittämistavasta käytettävyys ja asteikko. Kaikki riippuvuudet ja yksilöivä määritykset näkyvät korostettuina. Saat parhaan kokemuksen käyttöönotto valmiiksi Azure tilauksen ja työ sekä Azure Resurssienhallinta-mallin ratkaisun esiintymän. [Musiikin kaupan käyttöönoton Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux)löytyy tähän – valmis malli.

## <a name="availability-set"></a>Käytettävyys määrittäminen

Käytettävyys määrittää loogisesti laajuinen Azuren näennäiskoneiden fyysinen isännät- ja muita kuivatusohjelmat osia, kuten virtaa ja fyysinen verkko laitteisto. Käytettävyys joukot varmistaa, että aikana ylläpito, laitevirhe tai muu alaspäin aika, kaikki näennäiskoneiden toteutuminen. Käytettävyys määrittää voidaan lisätä Azure Resurssienhallinta-mallia käytetään Visual Studio Lisää uusi ohjattua resurssien tai kelvollinen JSON lisäämisestä mallina.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Saatavuus](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387)sisällä JSON-malli.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Käytettävyys määrittää on määritetty virtuaalikoneen resurssin ominaisuus. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [virtuaalikoneen saatavuus liitoksen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313)sisällä JSON-malli.


```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Käytettävyys sellaisena kuin se näkyy Azure-portaalista. Kunkin virtuaalikoneen sekä tietoja määritykset on kuvattu seuraavassa.

![Käytettävyys määrittäminen](./media/virtual-machines-linux-dotnet-core/aset.png)

Katso tarkempia tietoja käytettävyys joukot, [näennäiskoneiden käytettävyyden hallinta](./virtual-machines-linux-manage-availability.md). 

## <a name="network-load-balancer"></a>Verkon kuormituksen

Käytettävyys määrittäminen säädetään sovelluksen vikasietoa kuormituksen tasauspalvelun on monta sovelluksen esiintymää käytettävissä yhden verkko-osoite. Sovelluksen useita kertoja voidaan ylläpitää monta näennäiskoneiden, yksitellen, kuormituksen yhteydessä. Kun sovellus on käytettävissä, kuormituksen reitittää saapuvan pyynnön liitetty jäsenien. Kuormituksen tasauspalvelun voidaan lisätä Visual Studio Lisää uusi ohjattua resurssien tai lisäämällä oikein muotoiltu JSON resurssin Azure Resurssienhallinta-malliin.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Verkon kuormituksen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208)sisällä JSON-malli.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Koska sovelluksen malli on määritetty julkiseen IP-osoite Internet-osoite on liitetty kuormituksen. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [verkon kuormituksen liitoksen julkiseen IP-osoite](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221)sisällä JSON-malli.

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Azure-portaalista verkon kuormituksen tasauspalvelun yleiskatsaus näyttää liitoksen julkiseen IP-osoitteeseen.

![Verkon kuormituksen](./media/virtual-machines-linux-dotnet-core/nlb.png)

## <a name="load-balancer-rule"></a>Kuormituksen tasauspalvelun sääntö

Kuormituksen tasauspalvelun käytettäessä säännöt määritetään, jotka määrittävät, kuinka liikenne täsmätään tarkoitettu resurssien välillä. Esimerkki musiikkia Store-sovelluksen kanssa liikenne porttiin 80 julkiseen IP-osoite ja kaikki näennäiskoneiden portti 80 jaetaan. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Kuormituksen tasauspalvelun säännön](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270)sisällä JSON-malli.


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Näytä verkon kuormituksen tasauspalvelun säännön-portaalista.

![Verkon kuormituksen tasauspalvelun sääntö](./media/virtual-machines-linux-dotnet-core/lbrule.png)

## <a name="load-balancer-probe"></a>Kuormituksen tasauspalvelun näytteenottimen

Kuormituksen on myös valvoa kunkin virtuaalikoneen niin, että pyynnöt avautuvat vain järjestelmien suorittamiseen. Seuranta tapahtuu mukaan ennalta määritetyt portin vakion tutkitaan. Musiikin säilön käyttöönotto on määritetty tutkia kaikki mukana näennäiskoneiden 80 porttiin. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Kuormituksen tasauspalvelun tutkia](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257)sisällä JSON-malli.


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Kuormituksen tasauspalvelun keräysputken nähdä Azure-portaalista.

![Verkon kuormituksen tasauspalvelun näytteenottimen](./media/virtual-machines-linux-dotnet-core/lbprobe.png)

## <a name="inbound-nat-rules"></a>Saapuvan liikenteen säännöt NAT

Lataa tasaustoiminto käytettäessä säännöt täytyy laittaa paikkaa, jossa ei ole kuormituksen tasapainoinen access antaa kunkin Virtual Machine. Esimerkiksi kun luot SSH yhteyden kunkin virtuaalikoneen, tämä liikenne ei saa olla kuormitus tasataan, mieluummin ennalta määritetty liikerata määritettävä. ennalta määritetty polut määritetään saapuva NAT säännön resurssi. Käytä tämän resurssin, saapuvan tietoliikenteen voidaan määrittää yksittäisen näennäiskoneiden. 

Musiikin Store-sovelluksella 5000 aloittaen portin on nyt yhdistetty kunkin Virtual Machine SSH käytön 22 porttiin. `copyindex()` -Funktiota käytetään saapuvan portin kasvattaa siten, että toinen virtuaalikoneen vastaanottaa saapuvia portin 5001 kolmannen 5002, ja niin edelleen.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Saapuvan liikenteen NAT säännöt](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270)sisällä JSON-malli. 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Esimerkki saapuvan liikenteen NAT sääntö tarkastelu Azure-portaalissa. SSH NAT-sääntö on luotu käyttöönoton virtual kunkin tietokoneeseen.

![Saapuvan NAT sääntö](./media/virtual-machines-linux-dotnet-core/natrule.png)

Azure-verkon kuormituksen tarkempia tietoja on artikkelissa [Azure infrastruktuuripalvelut kuormituksen](./virtual-machines-linux-load-balance.md).

## <a name="deploy-multiple-vms"></a>Ottaa käyttöön useita VMs

Lopuksi saatavuus tai kuormituksen toimimaan tehokkaasti, useita näennäiskoneiden tarvitaan. Useita VMs voidaan ottaa käyttöön Azure Resurssienhallinta mallin kopio-funktion käyttäminen. Kopioi-funktion käyttäminen ei tarvitse määrittää näennäiskoneiden on rajallinen määrä, sen sijaan, että tämä arvo dynaamisesti saadaan käyttöönoton yhteydessä. Kopioi-funktio käyttää kerrat luomista ja kahvat käyttöönotto oikea määrä näennäiskoneiden ja niihin liittyvät resurssit.

Musiikin kaupan malli-malli parametri on määritetty, joka ottaa esiintymän määrä. Tätä numeroa käytetään koko mallin luotaessa näennäiskoneiden ja siihen liittyvät resurssit.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

Virtuaalikoneen, resurssin kopioi silmukan on annettu nimi ja esiintymät parametrin käytetään ohjaamaan tuloksena kopioiden lukumäärä määrä.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Virtuaalikoneen kopio-funktion](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300)sisällä JSON-malli. 


```none
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

Kopioi-toimintoa nykyisen iteraation niitä voi käyttää kanssa `copyIndex()` funktio. Kopioi indeksi-funktion arvo voidaan nimeksi näennäiskoneiden ja muita resursseja. Jos virtual machine kaksi esiintymää on otettu käyttöön, ne on esimerkiksi eri nimiä. `copyIndex()` Funktio voidaan virtuaalikoneen nimeen Luo yksilöllinen nimi. Esimerkki `copyindex()` nimeämiseen tarkoituksiin funktio voidaan näyttää virtuaalikoneen resurssi. Tässä kohdassa tietokonenimi on yhdistelmä `vmName` parametrin ja `copyIndex()` funktio. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Kopioi indeksi-funktion](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319)sisällä JSON-malli. 


```none
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

`copyIndex` -Funktiota on käytetty useita kertoja musiikkia kaupan otoksen mallissa. Resurssien ja funktioita käyttävien `copyIndex` uusimmat tietyn verkkokäyttöliittymä, kuormituksen tasauspalvelun sääntöjä, kuten virtuaalikoneen yksittäisen esiintymän ja jokin määräytyy funktiot. 

Lisätietoja kopio-funktiosta on artikkelissa [luominen Azure resurssien hallinnan resurssit useita kertoja](../resource-group-create-multiple.md).

## <a name="next-step"></a>Seuraava vaihe

<hr>

[Vaihe 4 - sovellusten käyttöön Azure Resurssienhallinta-mallit](./virtual-machines-linux-dotnet-core-5-app-deployment.md)