<properties
   pageTitle="Riippuvuudet Resurssienhallinta mallien | Microsoft Azure"
   description="Kerrotaan, miten voit määrittää yhden resurssin määräytyy toiselle resurssille varmistamiseksi resursseja on otettu käyttöön oikeassa järjestyksessä käyttöönoton aikana."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Riippuvuuksien määrittäminen Azure Resurssienhallinta-mallit

Tietyn resurssi voi olla muita resursseja, joka on oltava olemassa ennen resurssi on otettu käyttöön. Esimerkiksi SQL server on oltava olemassa ennen kuin yrität asentaa SQL-tietokantaan. Tätä yhteyttä määrittää yhden muun resurssin määräytyy resurssin merkitsemällä. Yleensä määrität riippuvuus **dependsOn** -elementin, mutta voit myös määrittää sen **viittaus** -toimintoa. 

Resurssienhallinta arvioi resurssien väliset riippuvuudet, ja ottaa käyttöön niiden riippuvaiset järjestyksessä. Kun resurssit eivät ole riippuvainen toistensa, Resurssienhallinta tunnistus ne rinnakkain.

## <a name="dependson"></a>dependsOn

DependsOn-osan avulla voit määrittää yhden tai useamman resurssin määräytyy resurssin sisällä malliin. Arvonsa voi olla resurssien nimet CSV-luettelo. 

Seuraavassa esimerkissä esitetään virtuaalikoneen asteikko sarja, joka määräytyy kuormituksen tasauspalvelun, VPN ja silmukka, joka luo tallennustilan useita tilejä. Nämä resurssit eivät näy seuraavassa esimerkissä, mutta ne on olemassa muualla malli.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Määritä riippuvuussuhteita niiden välille resurssi ja resurssit, jotka on luotu kopioi silmukan kautta, Aseta dependsOn osan silmukan nimi. Esimerkki-kohdassa [Luo resursseja Azure Resurssienhallinta useita kertoja](resource-group-create-multiple.md).

Vaikka voit olla kulmassa dependsOn avulla voit yhdistää resurssien suhteita, on tärkeää ymmärtää, miksi teet sitä, koska se saattaa olla vaikutusta käyttöönoton suorituskykyä. Esimerkiksi asiakirjaan, miten resurssit on yhdistetty toisiinsa, dependsOn ei ole oikea lähestymistapa. Et voi suorittaa kyselyä resursseja on määritetty dependsOn osaan käyttöönoton jälkeen. Käyttämällä dependsOn, voit mahdollisesti olla vaikutusta käytön aikana koska Resurssienhallinta Ota rinnakkain kaksi resursseja, joiden riippuvuuden. Asiakirjan resurssien suhteita, käytä sen sijaan [resurssin linkittämistä](resource-group-link-resources.md).

## <a name="child-resources"></a>Ali-resurssit

Resurssit-ominaisuuden avulla voit määrittää lapsen resurssit, jotka liittyvät määritettävään resurssi. Lapsen resursseja voi olla vain määritetyt syvyys viisi tasoa. On tärkeää muistaa implisiittinen riippuvuuden ei ole luotu ali-resurssin ja ylemmän tason resurssin. Lapsen resurssin käyttöön jälkeen ylemmän tason resurssi on ilmoitettava erikseen jäsenyys dependsOn-ominaisuus. 

Ylemmän tason kullekin resurssille hyväksyy vain tietyt resurssityypit lapsen resursseina. Hyväksytyt resurssityypit määritetään ylemmän tason resurssin [mallin rakenne](https://github.com/Azure/azure-resource-manager-schemas) . Lapsen resurssilaji nimi sisältää tyypin ylemmän tason resurssin nimi, kuten **Microsoft.Web/sites/config** ja **Microsoft.Web/sites/extensions** ovat molemmat **Microsoft.Web/sites**ali-resursseja.

Seuraavassa esimerkissä esitetään, SQL server- ja SQL-tietokantaan. Huomaa, että SQL-tietokantaan ja SQL server on määritetty eksplisiittinen riippuvuuden vaikka tietokanta on palvelimeen.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>Viite-funktio

[Viite-funktion](resource-group-template-functions.md#reference) avulla lausekkeen arvonsa johdettu JSON muu nimi ja arvon tietoparin tai runtime resurssit. Viittaus lausekkeiden määritellä implisiittisesti, yksi resurssi, riippuu siitä, toiseen. 

    reference('resourceName').propertyPath

Voit käyttää tämä elementti tai dependsOn osan voit määrittää riippuvuuksia, mutta sinun ei tarvitse käyttää sekä riippuvaiset samalle resurssille. Aina, kun se on mahdollista, vältä vahingossa tarpeettomat riippuvuuden implisiittinen viittaus avulla.

Lisätietoja on artikkelissa [viittaus-funktio](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Azure Resurssienhallinta-mallien luomisesta on artikkelissa [julkaisu malleja](resource-group-authoring-templates.md). 
- Saat kaikki käytettävissä olevat funktiot mallissa luettelo on [malli-Funktiot](resource-group-template-functions.md).

