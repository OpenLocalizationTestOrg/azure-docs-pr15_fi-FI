<properties
   pageTitle="Linkitetty mallit ja resurssin Manager | Microsoft Azure"
   description="Tässä artikkelissa käsitellään moduulit malli-ratkaisun luominen Azure Resurssienhallinta-mallin linkitetty mallien avulla. Voit siirtää parametrien arvot, määritä parametri-tiedosto ja dynaamisesti luodun URL-osoitteet."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Linkitetyn mallien avulla Azure resurssien hallinta

-Sisällä yhteen Azure Resurssienhallinta-malliin, voit linkittää johonkin toiseen malliin, jonka avulla voit Hajota joukkoon käyttöönoton kohdistettu tarkoitus kielikohtaiset malleja. Samoin kuin decomposing sovelluksen useita koodin luokkaan hajotus on etuja testaus, uudelleenkäyttöä ja luettavuutta.  

Voit siirtää parametrit tärkeimmät mallista ja linkitettyä mallia ja nämä parametrit voit yhdistää suoraan parametreja tai muuttujat näyttämiä puheluja malli. Linkitettyä mallia voit myös siirtää tuloksena saatava muuttuja takaisin lähde-mallin ottaminen käyttöön kaksisuuntainen tiedonsiirron mallien välillä.

## <a name="linking-to-a-template"></a>Linkittäminen lisääminen malliin

Voit luoda kaksi mallia lisäämällä tärkeimmät malli, joka osoittaa linkitettyä mallia käyttöönoton resurssiin välisen linkin. **TemplateLink** -ominaisuuden arvoksi URI ja linkitettyä mallia. Voit antaa parametriarvot linkitettyä mallia määrittämällä arvot suoraan mallin tai linkittämällä parametrin tiedostoon. Seuraavassa esimerkissä **Parametrit** -ominaisuuden määrittäminen parametrin arvon suoraan.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Resurssienhallinta-palvelu on voi käyttää linkitettyä mallia. Et voi määrittää paikallisen tiedoston tai tiedoston, joka on käytettävissä linkitettyä mallia lähiverkon vain. Voit kirjoittaa vain URI-arvo, joka sisältää **http** tai **https**. Yksi vaihtoehto on linkitettyä mallia tallennustilan tilin ja käyttää URI kyseisen kohteen, kuten seuraavassa esimerkissä.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Vaikka linkitettyä mallia on oltava ulkoisesti käytettävissä, se ei tarvitse on yleisesti saatavilla yleisölle. Voit lisätä mallin, jota voi käyttää tallennustilan tilin omistajan yksityisen-tilille. Luo sitten jaettua käyttöä allekirjoitus (SAS)-tunnuksen käyttämiseksi käyttöönoton aikana. Voit lisätä kyseisen SAS tunnuksen ja linkitettyä mallia URI. Katso ohjeet tallennustilan tilillä mallin määrittäminen ja luodaan Suojaussidokset-tunnuksen, [käyttöönotto resursseja Resurssienhallinta mallit ja PowerShellin Azure](resource-group-template-deploy.md) tai [Resurssienhallinta-mallit ja Azure CLI käyttöönotto resursseilla](resource-group-template-deploy-cli.md). 

Seuraavassa esimerkissä esitetään pääkohde-malli, jossa on linkki toiseen malliin. Linkitettyä mallia käytetään SAS-tunnuksen, joka on välitetty parametrina kanssa.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Vaikka tunnus on välitetty suojatun merkkijonona, linkitettyä mallia, mukaan lukien SAS-tunnuksen URI on kirjautunut resurssin ryhmän käyttöönottotoimintoja. Voit rajoittaa näyttäminen, Määritä tunnuksen vanhenemisen.

## <a name="linking-to-a-parameter-file"></a>Linkittäminen parametri-tiedosto

Seuraavassa esimerkissä **parametersLink** -ominaisuuden avulla linkin parametrin tiedostoon.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

Linkitetyn parametrin tiedoston URI-arvoa ei voi olla paikallisen tiedoston ja täytyy sisältää **http** tai **https**. Parametri tiedosto voi olla myös tarkasteltavana käyttöoikeuksien SAS-tunnuksen.

## <a name="using-variables-to-link-templates"></a>Muuttujien käyttäminen linkitettävän mallit

Edellisessä kohdassa käytettyä ilmeni koodattu URL-Osoitteen arvot malli-linkkejä. Tämän menetelmän saattaa toimia yksinkertainen malli, mutta se ei toimi oikein, kun käsittelet suuri joukko moduulit malleja. Sen sijaan voit luoda staattinen muuttuja, joka tallentaa perus tärkeimmät mallin URL-osoite ja dynaamisesti Luo URL-osoitteet linkitetyn malleja, perus URL-osoitteesta. Tämän menetelmän etuna on se voit helposti siirtää ja haarautuvat mallia, koska haluat muuttaa tärkeimmät mallin staattinen muuttuja. Tärkeimmät mallin välittää oikea URI koko hajonneen malli.

Seuraavassa esimerkissä esitetään, miten perus URL-Osoitteen avulla voit luoda linkitetyn mallit (**sharedTemplateUrl** ja **vmTemplate**) kaksi URL-osoitteet. 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Voit myös perus URL-Osoitteen hankkiminen nykyiseen malliin [deployment()](resource-group-template-functions.md#deployment) avulla ja muut mallit URL-Osoitteen hankkiminen samassa sijainnissa, avulla. Tämä vaihtoehto on hyödyllinen, jos mallin sijainti muuttuu (ehkä vuoksi versiotiedoissa) tai haluat välttää coding kiintolevyn mallitiedoston URL-osoitteet. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Ehdollisesti malleihin linkittäminen

Voit linkittää erilaisia malleja, lähi-parametrin arvon, jota käytetään muodostaa URI ja linkitettyä mallia. Tämä menetelmä toimii hyvin, kun haluat määrittää, jotka linkittää malli, jota käytetään käyttöönoton aikana. Voit esimerkiksi määrittää yhden olemassa olevan tallennustilan tilin malli ja toiseen malliin uusien tallennustila-tiliä.

Seuraavassa esimerkissä parametri tallennustilan tilin nimi ja määritä, onko tallennustilan tilin uuteen tai aiemmin luotuun parametri.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Voit luoda mallin, joka sisältää uuteen tai aiemmin luotuun-parametrin arvon URI muuttuja.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Antaisit käyttöönoton resurssin muuttujan arvo.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

URI korjaa mallille nimi **existingStorageAccount.json** tai **newStorageAccount.json**. Mallien luominen näiden URI.

Seuraavassa esimerkissä **existingStorageAccount.json** -malli.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Seuraavassa esimerkissä näkyy **newStorageAccount.json** -malli. Huomaa, että kuten olemassa olevan tallennustilan tilin mallin tilin tallennuspaikka palautetaan hyväksyttäväksi. Perusmuodon mallin toimii joko linkitettyä mallia.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Valmis Esimerkki

Seuraavassa esimerkissä mallit Näytä yksinkertaistettu linkitetyn mallien järjestyksen esitä useita tämän artikkelin käsitteitä. Se olettaa malleja on lisätty tallennustilan tilin samaan säilöön yleisen käytön käytöstä. Linkitettyä mallia välittää arvon takaisin **tulostaa** osan tärkeimmät malliin.

**Parent.json** -tiedosto sisältää seuraavat tiedot:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

**Helloworld.json** -tiedosto sisältää seuraavat tiedot:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
PowerShell-tunnuksen hankkiminen säilö ja käyttöönotto mallien:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

Azure CLI tunnuksen hankkiminen säilö ja ottaa käyttöön seuraavat koodin malleja. Tällä hetkellä sinun on määritettävä käyttöönoton nimi, joka sisältää SAS tunnuksen URI mallia käytettäessä.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Sinua kehotetaan antamaan parametrina SAS-tunnuksen. Tarvitset tunnuksen, jonka **?**etuliitteellä.

## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja määrittäminen resurssien käyttöönoton-tilaus on artikkelissa [Azure Resurssienhallinta malleja riippuvuuksien määrittäminen](resource-group-define-dependencies.md)
- Voit määrittää yhden resurssin mutta luoda monta esiintymää on artikkelissa [Azure resurssien hallinnan resurssit useita kertoja luominen](resource-group-create-multiple.md)
