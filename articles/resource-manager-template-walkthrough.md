<properties
   pageTitle="Resurssienhallinta mallin ongelmatilanteita | Microsoft Azure"
   description="Resurssien hallinnan mallin valmistelu basic Azure IaaS-arkkitehtuuri vaiheittainen vaiheista."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Resurssienhallinta mallin ongelmatilanteita

Ensimmäinen kysymyksiä mallia luotaessa on "miten Käynnistä?". Yksi aloittaa tyhjästä mallista, seuraavan perusrakenteen [yhtä aikaa muiden kanssa mallin artikkelissa](resource-group-authoring-templates.md#template-format)kuvatulla tavalla ja lisää resursseja, tarvittavat parametrit ja muuttujien. Aloita siirtymällä käyttää [pikaopas valikoima](https://github.com/Azure/azure-quickstart-templates) ja Etsi samanlaisia skenaariot yrität luoda tekemääsi on hyvä vaihtoehto. Voit yhdistää useita malleja tai muokata aiemmin luotua sopivaksi tietyn vaihtoehto. 

Voit tarkastella yleisiä infrastruktuurin etsiminen:

* Kaksi näennäiskoneiden, joka käyttää samaa tallennustilan tilin, ovat samat käytettävyys- ja saman aliverkon virtual verkkoon.
* Yksittäinen NIC ja AM IP-osoite kunkin virtuaalikoneen varten.
* Kuormituksen kuormituksen säännön porttiin 80 kanssa

![arkkitehtuuri](./media/resource-group-overview/arm_arch.png)

Tässä ohjeaiheessa esitellään koskevan ilmoituksen, että infrastruktuurin Resurssienhallinta mallin luominen. Lopullinen mallia, voit luoda perustuu pikaopas mallin nimeltä [2 VMs ladata tasaustoiminto ja kuormituksen säännöt](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/).

Mutta, joka on useita, voit luoda yhdellä kertaa, joten katsotaan seuraavaksi Luo tallennustilan tilin ja ota se käyttöön. Sen jälkeen, kun olet selvittänyt tallennustilan tilin luominen, voit lisätä muita resursseja ja viimeistele infrastruktuuri mallin käyttöön uudelleen.

>[AZURE.NOTE] Voit käyttää minkä tyyppisessä editorin mallin luotaessa. Visual Studio sisältää työkaluja, joilla mallin kehittämisen, mutta et tarvitse Visual Studio suorittamiseen tässä opetusohjelmassa. Katso opetusohjelma luomisesta Web App- ja SQL-tietokannan käyttöönotto Visual Studiossa avulla, [luominen ja käyttöönotto Azure resurssiryhmät Visual Studio kautta](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>Resurssienhallinta-mallin luominen

Malli on JSON-tiedosto, joka määrittää kaikkien resurssien otetaan käyttöön. Se myös avulla voit määrittää parametreja, jotka on määritetty käyttöönoton aikana muuttujat, jotka muut arvot ja lausekkeiden ja tulostaa käyttöönotosta rakennettava. 

Aloitetaan helpoin mallin:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Tiedoston tallentaminen **azuredeploy.json** (Huomaa, että mallin voi olla nimi, jonka haluat, että se on oltava json-tiedosto).

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen
**Resurssit** -osassa Lisää objekti, joka määrittää tallennustilan tilin alla kuvatulla tavalla. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Mutta joiden nämä ominaisuudet ja arvot näytetään. Ominaisuudet- **tyyppi**, **nimi**, **apiVersion**ja **sijainti** on vakio-osat, jotka ovat käytettävissä kaikissa resurssityypit. Lisätietoja yhteisten osien [resurssit](resource-group-authoring-templates.md#resources)-palvelussa. parametrin arvon, voit siirtää aikana käyttöönotto- ja **sijainnin** resurssiryhmän käyttämä sijainniksi on määritetty **nimi** . Tarkastellaan, miten voit määrittää **tyyppi** ja **apiVersion** alla olevissa osioissa.

**Ominaisuudet** -osan sisältää kaikki ominaisuudet, jotka ovat yksilöllisiä tietyn resurssilaji. Tässä osassa määritetään täsmälleen arvot vastaavat HYLLYTETTY toiminnon REST-Ohjelmointirajapinta, kyseisen resurssityyppi luomiseen. Kun tallennustilan tilin luominen, sinun on annettava **accountType**. Huomaa, että REST-toiminnon ominaisuudet-osan on myös **accountType** -ominaisuus ja sallitut arvot on kuvattu [REST API varten tallennustilan tilin luominen](https://msdn.microsoft.com/library/azure/mt163564.aspx) . Tässä esimerkissä tilin tyyppi on määritetty **Standard_LRS**, mutta voit määrittää jonkin muun arvon tai että käyttäjät voivat siirtää tilityyppi parametrina.

Nyt siirtyä takaisin **Parametrit** -osassa luodaan ja katso, miten voit määrittää tallennustilan tilin nimi. Voit lukea lisää käyttämisestä parametrien [Parametrit](resource-group-authoring-templates.md#parameters)-palvelussa. 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Tässä määrittämäsi parametrin merkkijonomuotoisen, johon haluat tallennustilan tilin nimi. Parametrin arvo toimitetaan mallin käyttöönoton aikana.

## <a name="deploying-the-template"></a>Käyttöönotto malli
On koko mallin tallennustilan uuden tilin luominen. Peruuta, kun malli on tallennettu **azuredeploy.json** tiedosto:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

On aivan muutamalla tavalla ottamaan mallia, kuten näet [resurssin käyttöönoton artikkelissa](resource-group-template-deploy.md). Ottamaan PowerShellin Azure mallin avulla:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Tai ottamaan käyttämällä Azure CLI mallin avulla:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Olet nyt ylpeä tallennustilan tilin omistajan!

Seuraavat vaiheet on Lisää kaikki tarvittavat käyttöön tässä opetusohjelmassa alkuun kuvattu arkkitehtuuri resurssit. Lisää nämä resurssit saman olet käyttänyt-mallissa.

## <a name="availability-set"></a>Käytettävyys määrittäminen
Tallennustilan tilin määrityksen jälkeen voit lisätä availably määritetty näennäiskoneiden. Tässä tapauksessa ei ole muita ominaisuuksia vaaditaan, jotta sen määritelmän on melko helppoa. Katso täydet ominaisuudet-osan [REST API luomisen käytettävyys määrittäminen](https://msdn.microsoft.com/library/azure/mt163607.aspx) siltä varalta, että haluat määrittää Päivitä toimialueen Laske- ja vika toimialueen Laske arvot.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Huomaa, että **nimi** on määritetty muuttuja-arvo. Tämän mallin käytettävyys joukon nimi on tarvittavat muutamalla eri paikoissa. Voit ylläpitää mallin välillä määrittämällä arvon kerran ja käyttämällä useita paikassa.

Voit määrittää **tyyppi** arvo sisältää resurssin tarjoajaan ja resurssin laji. Käytettävyys joukkojen resurssi-palvelu on **Microsoft.Compute** ja resurssin laji on **availabilitySets**. Voit hankkia käytettävissä resurssin palvelutarjoajien luetteloon suorittamalla seuraavan PowerShell-komennon:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Tai jos käytössäsi on Azure CLI, voit suorittaa seuraava komento:
```
    azure provider list
```
Koska, luot tämän ohjeaiheen tallennustilan tilit, näennäiskoneiden ja virtual verkko, jonka toimii kanssa:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Jos haluat nähdä tietyn palveluntarjoajasi resurssityyppejä, suorita PowerShell-komentoa:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Tai Azure CLI seuraava komento käytettävissä olevia tyyppejä palauttaa JSON-muodossa ja Tallenna tiedostoon.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Raportissa pitäisi näkyä **availabilitySets** yksi tyypit **Microsoft.Compute**kuluessa. Koko tyyppi on **Microsoft.Compute/availabilitySets**. Voit määrittää seuraavia resursseja, voit mallin nimi.

## <a name="public-ip"></a>Julkiseen IP
Määritä julkinen IP-osoite. Tarkista uudelleen [REST API julkiseen IP-osoitteiden](https://msdn.microsoft.com/library/azure/mt163590.aspx) ominaisuuksien määrittämiseen.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Se on määritetty **dynaamiseksi** , mutta voi määrittää sinun tai määrittäminen hyväksymään parametriarvo-arvoa. Siirtää arvon toimialueen nimen otsikkona mallin käyttäjien on käytössä.

Seuraavaksi tarkastellaan siitä, miten voit määrittää **apiVersion**. Voit määrittää arvo vastaa yksinkertaisesti, jota haluat käyttää luotaessa resurssin REST API-version. Näin on voi tarkastella resurssin tyypin REST API-ohjeista. Vaihtoehtoisesti voit suorittaa seuraavan PowerShell-komennon tietyn.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Joka palauttaa seuraavat arvot:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Voit tarkastella API-versioiden kanssa Azure CLI suorittamalla saman **azure tarjoajan Näytä** -komento näkyy aiemmin.

Kun luot uuden mallin, valitse API uusimman version.

## <a name="virtual-network-and-subnet"></a>VPN- ja aliverkon
Luo virtuaalisia verkon yhden aliverkon. Tarkista [REST API virtual verkkojen](https://msdn.microsoft.com/library/azure/mt163661.aspx) ominaisuuksien määrittämiseen.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Kuormituksen
Kun luot ulkoisen aukeaman kuormituksen. Koska tätä kuormituksen käyttää julkinen IP-osoite, on määritettävä riippuvuus **dependsOn** -osassa julkisen IP-osoite. Tämä tarkoittaa kuormituksen eivät tule käyttöön, ennen kuin julkinen IP-osoite on päättynyt käyttöönotto. Tämä riippuvuus määrittämättä saat virheen koska Resurssienhallinta yrittää ottaa rinnakkain resurssit ja yrittää määrittää kuormituksen julkiseen IP-osoite, jota ei ole vielä. 

Myös luoda Taustajärjestelmä osoite resurssivarantoon, pari saapuvan NAT säännöt RDP VMs kyselyjä ja lataa Vastatilin säännön tcp-näytteenottimen porttiin 80 tämän resurssin määrityksessä kanssa. Uloskuittauksen [Kuormituksen REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/mt163574.aspx) kaikki ominaisuudet.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Verkkokäyttöliittymä
Luo 2 verkkoliittymät, yhteen kunkin AM. Sen sijaan, että haluat sisällyttää verkkoliittymät kaksoisarvoja, voit käyttää [copyIndex()-funktion](resource-group-create-multiple.md) käytöstä kopioi silmukan (jota kutsutaan nicLoop) päälle ja luo numero verkkoliittymät määritysten mukaisesti `numberOfInstances` muuttujat. Verkkoliittymän määräytyy virtual verkko- ja kuormituksen luomista. Se käyttää VPN-luomista ja kuormituksen tasauspalvelun tunnus määritelty aliverkon voit määrittää kuormituksen tasauspalvelun osoite resurssivarantoon ja NAT saapuvan liikenteen säännöt.
Tarkista kaikki ominaisuudet [Verkkoliittymät REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/mt163668.aspx) .

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Virtuaalikoneen
Luo 2 näennäiskoneiden copyIndex() funktiota käyttämällä samoin kuin [verkkoliittymät](#network-interface)luomista.
AM luominen määräytyy tallennustilan tilin verkon asetettujen käyttöliittymän ja käytettävyyttä. Tämän AM luodaan marketplace-kuvasta, jotka on määritelty `storageProfile` ominaisuus - `imageReference` käytetään määrittämään kuva Publisherin, tarjous, tuote ja versio. Lopuksi diagnostiikan profiili on määritetty diagnostiikka käyttöön AM. 

Voit etsiä haluamasi ominaisuudet marketplace-kuva, noudata [Linux virtuaalikoneen kuvia](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) tai [Valitse Windows virtuaalikoneen kuvia](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) artikkelit.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Kuvien julkaisema **3 osapuolen toimittajista**, tarvitset määrittää uuden ominaisuuden nimi `plan`. Esimerkki löytyy [tämän mallin](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) pikaopas valikoimasta. 

Olet määrittäminen mallin resurssit.

## <a name="parameters"></a>Parametrit

Määrittää arvot, jotka voidaan määrittää, kun otat mallin parametrit-osassa. Määritä vain parametrien arvot, jotka mielestäsi pitäisi olla vaihteleva käyttöönoton aikana. Voit antaa oletusarvon parametrille, jota käytetään, jos jokin ei anneta käyttöönoton aikana. Voit myös määrittää sallittu arvo, kuten **imageSKU** -parametrin.

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Muuttujat

Voit määrittää ovat keskitetysti mallissa käytettävät arvot tai arvoja, jotka on rakennettu muita lausekkeita tai muuttujat, muuttujat-osassa. Muuttujaa käytetään usein yksinkertaistaa mallin syntaksi.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Malli on valmis! Voit vertailla mallin vastaan koko malli [pikaopas valikoima](https://github.com/Azure/azure-quickstart-templates) Valitse [2 VMs kanssa kuormituksen ja ladata tasaustoiminto malli](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules). Mallin voi olla hieman erilainen eri versioita käyttämällä perusteella. 

Voit ottaa mallin käyttöön uudelleen käyttämällä samoja komentoja käytit, kun otat tallennustilan tilin. Sinun ei tarvitse poistaa tallennustilan tilin ennen kuin otat uudelleen koska Resurssienhallinta ohittaa uudelleen luominen resurssit, jotka on jo olemassa ja ei ole muutettu.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Resurssienhallinta mallin Visualizer (ARMViz)](http://armviz.io/#/) on erinomainen työkalu voi esittää ARM-mallit-heti, kun ne ovat liian suuria ymmärtää lukemasta json-tiedosto.
- Lisätietoja mallin rakenne on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md).
- Lisätietoja otat mallin käyttöön, katso [käyttöönotto resurssiryhmä Azure Resurssienhallinta-malli](resource-group-template-deploy.md)
