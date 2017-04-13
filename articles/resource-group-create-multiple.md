<properties
   pageTitle="Ottaa käyttöön useita kertoja resurssien | Microsoft Azure"
   description="Kopiointi ja matriisien käyttäminen Azure Resurssienhallinta-mallin käydä useita kertoja resurssien käyttöönoton yhteydessä."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Luo resursseja useita kertoja Azure resurssien hallinta

Tässä ohjeaiheessa kerrotaan, miten käytöstä Azure Resurssienhallinta-mallin luominen resurssin useita kertoja.

## <a name="copy-copyindex-and-length"></a>Kopioi, copyIndex ja pituus

Voit luoda useita kertoja resurssin sisällä voit määrittää **Kopioi** objekti, joka määrittää, kuinka monta kertaa käytöstä. Kopioi on seuraavanlainen:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Voit käyttää iteraation nykyarvo **copyIndex()** -funktiota, kuten alla olevassa ketjutettu-funktion sisällä.

    [concat('examplecopy-', copyIndex())]

Luotaessa useita resursseja matriisista arvoja voit määrittää määrä **pituus** -funktio. Kuin pituus-funktion parametri on taulukko.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Nimi-kentän arvoksi indeksi

Voit käyttää kopion toiminto luo resurssi, joka nimetään yksilöllisesti kasvavia indeksin perusteella useita kertoja. Haluat esimerkiksi lisätä on yksilöllinen numero, joka on otettu käyttöön resurssien nimien loppuun. Ottaa käyttöön kolme nimeltä sivustoista:

- examplecopy 0
- examplecopy-1
- examplecopy-2.

Käytä seuraavaa mallia:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Siirtymä indeksiarvoa

Huomaat edellisessä esimerkissä, indeksiarvon kulkee nolla 2. Voit siirtymä indeksiarvon, voit siirtää arvon **copyIndex()** -funktiota, kuten **copyIndex(1)**. Iteraatioita suorittamiseen on edelleen määritetty kopio-osaan, mutta copyIndex arvo korvataan määritetyllä arvolla. Mallilla sama kuin edellisessä esimerkissä, mutta määrittäminen **copyIndex(1)** käyttöön kolme nimetyt sivustoista:

- examplecopy-1
- examplecopy 2
- examplecopy-3

## <a name="use-copy-with-array"></a>Kopioi käyttäminen matriisi
   
Kopiointi on hyödyllistä, kun käsittelet taulukoita, koska voit käydä läpi jokainen matriisin osa. Ottaa käyttöön kolme nimeltä sivustoista:

- examplecopy Contoso
- examplecopy Fabrikam
- examplecopy Coho

Käytä seuraavaa mallia:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Voit määrittää kopioiden määrä kuin taulukon pituuden arvo. Voi esimerkiksi matriisin luominen useita arvoja ja sitten Välitä parametrin arvon, joka määrittää, kuinka monta taulukon elementtien otetaan käyttöön. Siinä tapauksessa voit määrittää kopioiden määrä, ensimmäisen esimerkin mukaisesti. 

## <a name="depending-on-resources-in-a-loop"></a>Silmukan resurssien mukaan

Voit määrittää, että resurssin käyttöön jälkeen toiselle resurssille **dependsOn** -osan avulla. Kun haluat ottaa resurssi, joka määräytyy resurssien silmukassa sivustokokoelman, voit nimetä kopioi silmukan **dependsOn** -osaan. Seuraavassa esimerkissä esitetään 3 tallennustilan tilit ottamisesta käyttöön ennen kuin otat virtuaalikoneen. Koko virtuaalikoneen määritystä ei ole näkyvissä. Huomaa, että kopioi elementti on **nimi** **storagecopy** asettaminen ja **dependsOn** -elementin näennäiskoneiden on määritetty myös **storagecopy**.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Sisäkkäiset resurssin toistoasetuksia

Et voi käyttää kopioi silmukan sisäkkäisiä resurssin. Jos haluat luoda resurssi, joka määritetään yleensä kuin sisäkkäisiä sisällä toiselle resurssille useita kertoja, sinun on sijaan Luo ylimmän tason resurssiksi resurssi ja määrittää ylemmän tason resurssin **laji** - ja **nimi** -ominaisuuksien avulla yhteyteen.

Oletetaan esimerkiksi, että yleensä Määritä tietojoukko Data Factory sisäkkäisiä resurssiin.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Voit luoda useita kertoja tietojoukkoja haluat mallin muuttaminen alla kuvatulla tavalla. Ilmoitus täysin hyväksytty-tyyppi ja nimi sisältää tietojen factory nimi.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Luo useita kertoja, kun kopioi ei toimi

Voit käyttää vain **Kopioi** resurssityypit, ei sisällä resurssilaji ominaisuudet. Tämä saattavat aiheuttaa ongelmia puolestasi, kun haluat luoda useita kertoja jotakin, mitä resurssi kuuluu. Voit luoda useita tietojen levyjä Virtual Machine on käytetty vaihtoehto. Et voi käyttää **Kopioi** tiedot-levyjen kanssa, koska **dataDisks** virtuaalikoneen ei omassa resurssin laji-ominaisuus. Sen sijaan voit luoda matriisi, niin monta tietojen levyjen tulee on, ja välittää tietoja levyä, jos haluat luoda todellinen määrä. Virtuaalikoneen-määrityksessä käytät vain määrä elementtejä, jotka haluat todella käyttämistä matriisin **kestää** -funktio.

Tätä mallia koko esimerkki on Näytä mallin [luominen ja tietojen levyjen dynaaminen valinta AM](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) .

Artikkelin olennaisista Kohdista käyttöönoton mallin näkyvät jäljempänä. Paljon malli on poistettu Korosta luomista dynaamisesti tietojen levyjen useita osia. Huomaa parametrin **numDataDisks** , jonka avulla voit välittää levyjä luomista määrä. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Seuraavat vaiheet
- Jos haluat lisätietoja mallin osiin, katso [Yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](./resource-group-authoring-templates.md).
- Saat kaikki toiminnot, voit käyttää mallissa [Azure Resurssienhallinta mallin Funktiot](./resource-group-template-functions.md).
- Lisätietoja mallin ottamisesta käyttöön on artikkelissa [Azure Resurssienhallinta mallilla sovelluksen käyttöönotto](resource-group-template-deploy.md).
