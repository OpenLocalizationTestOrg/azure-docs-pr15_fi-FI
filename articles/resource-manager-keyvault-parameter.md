<properties
   pageTitle="Avaimen säilö salaisuus Resurssienhallinta mallilla | Microsoft Azure"
   description="Voit siirtää salaisuus avaimen säilöstä parametrina käyttöönoton aikana."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Siirtää suojatun arvot käyttöönoton aikana

Kun haluat siirtää suojatun arvo (kuten salasanaa) parametrina käyttöönoton aikana, voit tallentaa arvon salaisuus [Azure avaimen säilö](./key-vault/key-vault-whatis.md) ja muiden Resurssienhallinta-mallien arvo viittaa. Toiminta viittaa sisällyttäminen malliin, jotta toiminta on määritetty ei koskaan ja sinun ei tarvitse manuaalisesti Anna arvo toiminta aina, kun otat resurssit. Määritä, ketkä käyttäjät tai palvelun ansaitun käyttää toiminta.  

## <a name="deploy-a-key-vault-and-secret"></a>Avaimen säilö ja salaisuus käyttöönotto

Luo avaimen säilö, joka voidaan viitata muista Resurssienhallinta-malleista, sinun on määritettävä **enabledForTemplateDeployment** -ominaisuuden arvoksi **true**ja käyttöoikeuden myöntäminen on käyttäjän tai palvelu, joka suoritetaan, joka viittaa toiminta käyttöönoton lyhennyksen.

Lisätietoja avaimen säilö ja salaisuus käyttöönotto on artikkelissa [avaimen säilö rakenteen](resource-manager-template-keyvault.md) ja [avaimen säilö salainen rakenne](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Salainen, jonka tunnus on staattinen viittaus

Voit viitata toiminta-parametrit-tiedostossa, joka välittää arvot mallin. Toiminta viitata kulkeva avaimen säilö resurssitunnus ja toiminta nimi. Tässä esimerkissä avaimen säilö toiminta on oltava olemassa ja käytössäsi on staattinen arvo sen resurssitunnus.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Koko parametri-tiedoston voi näyttää tältä:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Parametri, joka hyväksyy toiminta on oltava **securestring**. Seuraavassa esimerkissä on malli, joka käyttää SQL Serveriä, joka edellyttää järjestelmänvalvojan salasanan asianmukainen osia.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Viittaus dynaamisen tunnuksella salaisuus

Voit siirtää avaimen säilö toiminta staattinen resurssitunnus osoittanut edellisessä osassa. Kuitenkin joissakin tilanteissa haluat viitata avaimen säilö salaisuus, joka vaihtelee nykyinen käyttöönoton perusteella. Et voi siinä tapauksessa koodata resurssitunnus parametrit-tiedostossa. Valitettavasti et voi dynaamisesti luoda resurssitunnus-parametrit-tiedoston koska mallin lausekkeet eivät ole sallittuja parametrit-tiedostossa.

Voit luoda dynaamisesti avaimen säilö salaisuus resurssitunnus, sinun on siirrettävä resurssi on toiminta sisäkkäisiä malliin. Perusmuodon mallisi Lisää sisäkkäisiä malli ja välittää parametri, joka sisältää dynaamisesti luodun resurssitunnus.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Seuraavat vaiheet

- Yleisiä tietoja avaimen vaults artikkelissa [Azure avaimen säilö käytön aloittaminen](./key-vault/key-vault-get-started.md).
- Saat lisätietoja avaimen säilö käyttämisestä Virtual Machine [suojausasiat Azure Resurssienhallinta](best-practices-resource-manager-security.md).
- Valmis esimerkkejä viittaava avaimen tietoja [Avaimen säilö](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)esimerkkejä.

