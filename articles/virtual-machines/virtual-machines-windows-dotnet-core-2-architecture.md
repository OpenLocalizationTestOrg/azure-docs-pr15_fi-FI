<properties
   pageTitle="Käyttöönotto Laske resurssien Azure Resurssienhallinta malleja | Microsoft Azure"
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

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Sovelluksen arkkitehtuuri Azure Resurssienhallinta malleja

Kun kehittäminen Azure resurssien hallinnan käyttöönotto, Laske vaatimukset on yhdistettävä Azure resursseja ja palveluja. Jos jokin sovellus koostuu useita http päätepisteet, tietokannan ja välimuistiin palvelun tiedot, Azure resurssit, jotka isännöivät kunkin osista on voidaan Rationalized-opas. Musiikin Store-sovelluksen malli sisältää esimerkiksi virtual tietokoneeseen nykyisessä web-sovelluksen ja SQL-tietokantaan, joka isännöi Azure SQL-tietokantaan. 

Tämän asiakirjan tiedot siitä, miten musiikkia kaupan Laske resurssit on määritetty otoksen Azure Resurssienhallinta-mallissa. Kaikki riippuvuudet ja yksilöivä määritykset näkyvät korostettuina. Saat parhaan kokemuksen käyttöönotto valmiiksi Azure tilauksen ja työ sekä Azure Resurssienhallinta-mallin ratkaisun esiintymän. [Musiikin säilön käyttöönotto Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)löytyy tähän – valmis malli.

## <a name="virtual-machine"></a>Virtuaalikoneen

Musiikin Store-sovellus on web-sovelluksen, jossa asiakkaiden voit selata ja ostaa musiikkia. Internetissä on myös useita Azure palvelut, joita voit isännöidä verkkosovellusten tässä esimerkissä käytetään Virtual Machine. Esimerkki musiikkia Store-mallin avulla virtual machine on otettu käyttöön, verkkopalvelimen asentaminen ja musiikkia Store-sivuston asentanut ja määrittänyt. Tässä artikkelissa vuoksi yksityiskohtainen vain näennäiskoneiden käyttöönottoon. Verkkopalvelin ja sovelluksen kokoonpanon yksityiskohtainen myöhemmin artikkelin.

Virtual machine voidaan lisätä mallille Visual Studio Lisää uusi resurssi ohjatun toiminnon avulla tai lisäämällä kelvollinen JSON käyttöönotto-malliin. Virtual machine otettaessa useita liittyvät resurssit tarvitaan. Jos Visual Studion avulla voit luoda mallia, näistä resursseista on luonut puolestasi. Jos luominen manuaalisesti mallia, näistä resursseista on lisätty ja määrittänyt.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Virtuaalikoneen JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285)sisällä JSON-malli.

```none
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

Kun tiedot on käytössä, virtuaalikoneen ominaisuudet näkevät Azure-portaalissa.

![Virtuaalikoneen](./media/virtual-machines-windows-dotnet-core/vm-win.png)

## <a name="storage-account"></a>Tallennustilan tilin

Tallennustilan-tileillä on monta tallennusasetukset ja ominaisuuksia. Azure Virtual koneet kontekstin tallennustilan tilin pitää virtual kiintolevyillä virtuaalikoneen ja kaikki tiedot levyjä. Musiikin Store-malli sisältää tallennustilan tilien pitoon kunkin virtuaalikoneen virtual kiintolevyä käyttöönotto. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin- [Tallennustilan tilin](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98)sisällä JSON-malli.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Tallennustilan-tili on Liitä sisällä virtuaalikoneen Resurssienhallinta mallin ilmoituksen virtual koneen kanssa. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [virtuaalikoneen ja tallennustilaa tilin liitos](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321)sisällä JSON-malli.

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Käyttöönoton jälkeen tallennustilan tilin voi tarkastella Azure-portaalissa.

![Tallennustilan tilin](./media/virtual-machines-windows-dotnet-core/storacct-win.png)

Tallennustilan tilin blob säilöön valitsemalla voidaan näyttää kunkin virtuaalikoneen mallin käyttöön virtual kiintolevylle tiedosto.

![Virtuaalinen kiintolevyillä](./media/virtual-machines-windows-dotnet-core/vhd-win.png)

Lisätietoja Azure-tallennustilan [Azuren tallennustilaan](https://azure.microsoft.com/documentation/services/storage/)ohjeissa.

## <a name="virtual-network"></a>VPN

Jos virtual tietokone vaatii sisäinen verkko, kuten mahdollisuuden pitää yhteyttä muihin näennäiskoneiden ja Azure resursseja, Virtual Azure-verkko on pakollinen.  Virtuaalinen verkon Soita virtuaalikoneen käytettävissä Internetin välityksellä. Julkinen connectivity edellyttää julkiseen IP-osoite, joka on esitetty myöhemmin sarjassa.

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [VPN ja aliverkosta](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126)sisällä JSON-malli.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
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
  }
}
```

Azure-portaalista virtual verkon näyttää seuraavassa kuvassa. Huomaa, että kaikki mallin käyttöön näennäiskoneiden liitetyt virtual verkkoon.

![VPN](./media/virtual-machines-windows-dotnet-core/vnet-win.png)

## <a name="network-interface"></a>Verkkokäyttöliittymä

 Verkkoliittymän muodostaa virtual machine virtual verkon, tarkemmin aliverkon, joka on määritetty virtual verkossa. 
 
 Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Verkkoliittymän](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156)sisällä JSON-malli.
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Virtuaalikoneen kullekin resurssille sisältää verkkoprofiilia. Verkkoliittymän on liitetty virtuaalikoneen profiiliin.  

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Virtuaalikoneen Verkkoprofiilin](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330)sisällä JSON-malli.


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Azure-portaalista verkkokäyttöliittymä näyttää seuraavassa kuvassa. Sisäinen IP-osoite ja virtuaalikoneen suhteen näkevät käyttöliittymän verkkoresurssin.

![Verkkokäyttöliittymä](./media/virtual-machines-windows-dotnet-core/nic-win.png)

Lisätietoja Azure Virtual verkkojen [Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/)ohjeissa.

## <a name="azure-sql-database"></a>Azure SQL-tietokanta

Lisäksi virtual koneen isännöidä musiikkia Store-verkkosivustoa Azure SQL-tietokanta on otettu käyttöön isännöimiseen musiikkia myymälän tietokantaan. Azure SQL-tietokanta tähän käytön etuna on näennäiskoneiden toisen joukon ei edellytä, että asteikko ja käytettävyyden muodostanut palveluun.

Azure SQL-tietokanta voidaan lisätä Visual Studio Lisää uusi resurssi toiminnon, tai lisäämällä kelvollinen JSON mallia. SQL Server-resurssi on käyttäjänimi ja salasana, joka on myönnetty järjestelmänvalvojan oikeudet SQL-esiintymässä. Lisäksi SQL palomuurin resurssi on lisätty. Oletusarvon mukaan ylläpidettävä Azure sovellukset ovat pysty muodostamaan yhteyttä SQL-esiintymässä. Salli ulkoisen sovelluksen esimerkiksi SQL Server Management Studiossa SQL-esiintymään palomuurin varten on määritettävä. Musiikin kaupan esittely, vuoksi oletusarvo-määritys on kunnossa. 

Tätä linkkiä nähdäksesi Resurssienhallinta mallin – [Azure SQL-DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379)sisällä JSON-malli.


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Näkymä, SQL server- ja MusicStore tietokannan tarkastelu Azure-portaalissa.

![SQL Server](./media/virtual-machines-windows-dotnet-core/sql-win.png)

Lisätietoja Azure SQL-tietokannan käyttöönotto [Azure SQL-tietokannan](https://azure.microsoft.com/documentation/services/sql-database/)ohjeissa.

## <a name="next-step"></a>Seuraava vaihe

<hr>

[Vaihe 2 – käytön ja turvallisuuden Azure Resurssienhallinta-mallit](./virtual-machines-windows-dotnet-core-3-access-security.md)
