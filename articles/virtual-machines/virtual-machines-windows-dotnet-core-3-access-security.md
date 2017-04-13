<properties
   pageTitle="Käytön ja turvallisuuden Azure Resurssienhallinta-mallien | Microsoft Azure" 
   description="Azure virtuaalikoneen DotNet Core-opetusohjelma"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Käytön ja turvallisuuden Azure Resurssienhallinta-malleissa

Sovellusten ylläpidettävä Azure todennäköisesti on oltava access Internetin tai VPN / Express reitin Azure-yhteyttä. Musiikin kaupan sovelluksen esimerkissä kanssa sivuston otetaan käyttöön Internetissä julkiseen IP-osoite. Vahvistettu Accessin yhteydet-sovellukseen ja virtuaalikoneen resursseja itse suojata. Verkon käyttöoikeusryhmän mukana toimitetaan access-suojauksen. 

Tämän asiakirjan tiedot siitä, miten musiikkia Store-sovellus on suojattu otoksen Azure Resurssienhallinta mallia. Kaikki riippuvuudet ja yksilöivä määritykset näkyvät korostettuina. Saat parhaan kokemuksen käyttöönotto valmiiksi Azure tilauksen ja työ sekä Azure Resurssienhallinta-mallin ratkaisun esiintymän. [Musiikin säilön käyttöönotto Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)löytyy tähän – valmis malli.


## <a name="public-ip-address"></a>Julkiseen IP-osoite

Antaa yleisölle Azure resurssi, voidaan resurssille julkiseen IP-osoite. Julkinen IP-osoite voidaan määrittää staattinen tai dynaaminen IP-osoite. Jos dynaaminen osoitetta käytetään ja virtuaalikoneen pysäytetään ja Varaus poistettu, osoitteet on poistettu. Jos tietokoneen aloitetaan uudelleen, se voidaan määrittää eri julkiseen IP-osoite. Jos haluat estää IP-osoitteen muuttaminen, voidaan varattu IP-osoite. 

Julkinen IP-osoite voidaan lisätä ohjattua Visual Studio Lisää uusi resurssien Azure Resurssienhallinta-malliin tai lisäämällä kelvollinen JSON malliin. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Julkisen IP-osoitteen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110)sisällä JSON-malli.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIpAddressName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "public-ip"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Julkisen IP-osoitteen voi olla Virtual verkkosovittimen tai kuormituksen tasauspalvelun. Tässä esimerkissä koska musiikkia Store-sivustossa on useita näennäiskoneiden saapuva lataaminen julkiseen IP-osoite on liitetty ladata tasaustoiminto.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [julkisen IP-osoitteen liitoksen kuormituksen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211)sisällä JSON-malli.

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Julkinen jossa se näkyy Azure-portaalista IP-osoite. Huomaa, että julkinen IP-osoite on yhdistetty kuormituksen ja ei virtual machine. Verkon kuormituksen tasoitusmääritykset on kuvattu tämän sarjan seuraava asiakirja.

![Julkiseen IP-osoite](./media/virtual-machines-windows-dotnet-core/pubip-win.png)

Lisätietoja Azure julkisten IP-osoitteiden on artikkelissa [Azure IP-osoitteet](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Verkko-käyttöoikeusryhmän

Kun access on muodostettu Azure resurssien käyttöoikeuden pitäisi olla rajoitettu. Azure-virtuaalikoneissa suojattu käyttö tehdään verkon käyttöoikeusryhmän avulla. Musiikin kaupan sovelluksen esimerkissä kanssa kaikki käyttöä virtuaalikoneen on rajoitettu lukuun ottamatta http-käyttö portti 80 ja porttia 3389 RDP käytön. Verkon käyttöoikeusryhmän voidaan lisätä ohjattua Visual Studio Lisää uusi resurssien Azure Resurssienhallinta-malliin tai lisäämällä kelvollinen JSON malliin.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Verkon käyttöoikeusryhmän](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57)sisällä JSON-malli.

```none
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('networkSecurityGroup')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-security-group"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
},
```

Tässä esimerkissä verkko-käyttöoikeusryhmän on Liitä ilmoitettu VPN-resurssin aliverkon objektin kanssa. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [verkon käyttöoikeusryhmän liitoksen VPN](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143)sisällä JSON-malli.


```none
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
]
```

Seuraavassa on esimerkiksi näyttää verkko-käyttöoikeusryhmän Azure-portaalista. Huomaa, että NSG voidaan yhdistää aliverkon ja / tai verkon käyttöliittymän. Tässä tapauksessa NSG on liitetty aliverkon. Tässä määrityksessä saapuvan liikenteen säännöt koskevat kaikki näennäiskoneiden aliverkon yhteydessä.

![Verkko-käyttöoikeusryhmän](./media/virtual-machines-windows-dotnet-core/nsg-win.png)

Katso tarkempia tietoja verkko-käyttöoikeusryhmät, [Mikä on verkon käyttöoikeusryhmän]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Seuraava vaihe

<hr>

[Vaihe 3: saatavuudesta ja Azure Resurssienhallinta-mallit-asteikkoa](./virtual-machines-windows-dotnet-core-4-availability-scale.md)
