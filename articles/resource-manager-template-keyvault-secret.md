<properties
   pageTitle="Salainen avaimen säilöön Resurssienhallinta-malli | Microsoft Azure"
   description="Näyttää Resurssienhallinta rakenteen käyttöönotto avaimen säilö tietoja mallin avulla."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Avaimen säilö salainen mallin rakenne

Luo salainen, joka on tallennettu avaimen säilö. Tämän resurssin laji usein on otettu käyttöön [avaimen säilö](resource-manager-template-keyvault.md)lapsen se resurssiksi.

## <a name="schema-format"></a>Rakenteen muoto

Luo avaimen säilö salaisuus lisäämällä seuraavan rakenteen malliin. Toiminta voidaan määrittää joko lapsen resurssille avaimen säilö tai ylimmän tason resurssiksi. Voit määrittää sen lapsen resurssiksi, kun avaimen säilö on otettu käyttöön samaan malliin. Haluat määrittää toiminta ylimmän tason resurssin avaimen säilö ei ole otettu käyttöön sama mallin tai kun haluat luoda useita tietoja resurssin laji-toistuminen. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Arvot

Seuraavissa taulukoissa kuvataan arvot, jotka on määritelty määrittämiseen.

| Nimi | Arvo |
| ---- | ---- | 
| tyyppi | Luettelo<br />Pakollinen<br />**tietoja** (kun käyttöön avaimen säilö lapsen se resurssiksi) tai<br /> **Microsoft.KeyVault/vaults/secrets** (kun ylimmän tason resurssiksi otettu käyttöön)<br /><br />Resurssin laji luomiseen. |
| apiVersion | Luettelo<br />Pakollinen<br />**2015-06-01** tai **2014 – 12-19-esikatselu**<br /><br />API-versio, jonka avulla voi luoda resurssin. | 
| Nimi | Merkkijono<br />Pakollinen<br />Yksittäisen sanan, kun käyttöön lapsen resurssiksi avaimen säilö, tai muodossa **{avain-säilöön-name} / {nimi salaisuus}** kun ylimmän tason resurssiksi lisääminen aiemmin luotuun avaimen säilö käyttöön.<br /><br />Luo toiminta nimi. |
| Ominaisuudet: | Objektin<br />Pakollinen<br />[objektin ominaisuudet](#properties)<br /><br />Objekti, joka määrittää arvon, Luo toiminta. |
| dependsOn | Matriisi<br />Valinnainen<br />Pilkuilla erotettu luettelo resurssin nimiä tai resurssin yksilöivää tunnistetta.<br /><br />Resurssien tätä linkkiä määräytyy sivustokokoelman. Jos samaa mallia avaimen säilö, toiminta on otettu käyttöön, Sisällytä avaimen säilö nimi tämä elementti, jotta se on otettu käyttöön ensin. |

<a id="properties" />
### <a name="properties-object"></a>objektin ominaisuudet

| Nimi | Arvo |
| ---- | ---- | 
| arvo | Merkkijono<br />Pakollinen<br /><br />Voit tallentaa avaimen säilö salainen arvo. Kun kulkeva tämän ominaisuuden arvoa, käytä tyyppi **securestring**parametri.  |

    
## <a name="examples"></a>Esimerkkejä

Ensimmäinen ottaa käyttöön salaisuus avaimen säilö lapsen se resurssiksi.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Toinen esimerkki ottaa käyttöön toiminta ylimmän tason resurssiksi, joka on tallennettu aiemmin avaimen säilö.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Seuraavat vaiheet

- Yleisiä tietoja avaimen vaults artikkelissa [Azure avaimen säilö käytön aloittaminen](./key-vault/key-vault-get-started.md).
- Esimerkki viittaava avaimen säilö salaisuus otettaessa mallit-kohdassa [siirtää suojatun arvot käyttöönoton aikana](resource-manager-keyvault-parameter.md).


