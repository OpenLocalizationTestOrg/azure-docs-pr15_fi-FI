<properties
   pageTitle="Resurssienhallinta mallin avaimen säilö | Microsoft Azure"
   description="Näyttää Resurssienhallinta rakenteen käyttöönoton avaimen vaults mallin avulla."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Avaimen säilö mallin rakenne

Luo avaimen säilö.

## <a name="schema-format"></a>Rakenteen muoto

Voit luoda avaimen säilö, lisää seuraavan rakenteen mallin resurssit-osa.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Arvot

Seuraavissa taulukoissa kuvataan arvot, jotka on määritelty määrittämiseen.

| Nimi | Arvo |
| ---- | ---- | 
| tyyppi | Luettelo<br />Pakollinen<br />**Microsoft.KeyVault/vaults**<br /><br />Resurssin laji luomiseen. |
| apiVersion | Luettelo<br />Pakollinen<br />**2015-06-01** tai **2014 – 12-19-esikatselu**<br /><br />API-versio, jonka avulla voi luoda resurssin. | 
| Nimi | Merkkijono<br />Pakollinen<br />Nimi, joka on yksilöllinen Azure.<br /><br />Voit luoda avaimen säilö nimi. Harkitse [uniqueString](resource-group-template-functions.md#uniquestring) -funktion avulla ja että nimeämiskäytäntöä yksilöivä nimi, kuten alla olevassa esimerkissä. |
| sijainti | Merkkijono<br />Pakollinen<br />Avaimen vaults kelvollinen alue. Kelvollinen alueet-kohdassa [Tuetut alueet](resource-manager-supported-services.md#supported-regions).<br /><br />Alueen isännöimiseen avaimen säilö. |
| Ominaisuudet: | Objektin<br />Pakollinen<br />[objektin ominaisuudet](#properties)<br /><br />Objekti, joka määrittää avaimen säilö luomiseen. |
| resurssit | Matriisi<br />Valinnainen<br />Sallitut arvot: [avaimen säilö salainen resurssit](resource-manager-template-keyvault-secret.md)<br /><br />Lapsen resurssit avaimen säilö. |

<a id="properties" />
### <a name="properties-object"></a>objektin ominaisuudet

| Nimi | Arvo |
| ---- | ---- | 
| enabledForDeployment | Totuusarvo<br />Valinnainen<br />**Tosi** tai **Epätosi**<br /><br />Määrittää, jos säilö käytössä virtuaalikoneen tai palvelun kangasta käyttöönottoa varten. |
| enabledForTemplateDeployment | Totuusarvo<br />Valinnainen<br />**Tosi** tai **Epätosi**<br /><br />Määrittää, jos säilö käytössä Resurssienhallinta mallin ominaisuuksissa käytettäviksi. Lisätietoja on artikkelissa [siirtää suojatun arvot käyttöönoton aikana](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Totuusarvo<br />Valinnainen<br />**Tosi** tai **Epätosi**<br /><br />Määrittää, jos säilö on otettu käyttöön aseman salaus. |
| tenantId | Merkkijono<br />Pakollinen<br />**Yleisesti yksilöllinen tunnus**<br /><br />Tilauksen vuokraajan tunnus. Voit hakea ne [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) PowerShell-cmdlet-komennon tai **azure-tili Näytä** Azure CLI-komentoa. |
| accessPolicies | Matriisi<br />Pakollinen<br />[accessPolicies objekti](#accesspolicies)<br /><br />Matriisin enintään 16 objekteja, jotka Määritä käyttäjän tai palvelun lyhennys oikeudet. |
| tuote | Objektin<br />Pakollinen<br />[tuote-objekti](#sku)<br /><br />Avaimen säilö olevat versiot. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies objekti

| Nimi | Arvo |
| ---- | ---- | 
| tenantId | Merkkijono<br />Pakollinen<br />**Yleisesti yksilöllinen tunnus**<br /><br />Azure Active Directory-vuokraajan sisältävä access-käytännön **objektitunnus** vuokraajan tunnus |
| Objektitunnus | Merkkijono<br />Pakollinen<br />**Yleisesti yksilöllinen tunnus**<br /><br />Azure Active Directory-käyttäjän tai palvelu, joka on pääsy säilö lyhennys objektitunnus. Voit noutaa arvon [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) tai [Hae AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) PowerShellin cmdlet-komennot tai **azure ad-käyttäjä** tai **azure ad-sp** Azure CLI-komentoja. |
| käyttöoikeudet | Objektin<br />Pakollinen<br />[käyttöoikeudet-objekti](#permissions)<br /><br />Valitse tämä säilö Active Directory-objektin käyttöoikeudet. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions objekti

| Nimi | Arvo |
| ---- | ---- | 
| näppäimet | Matriisi<br />Pakollinen<br />**kaikki** **varmuuskopion** **luominen**, **salauksen**, **poistaminen**, **salata**, **Hae**, **tuominen**, **luettelon**, **palauttaa**, **Kirjaudu**, **unwrapkey**, **päivittää**, **Tarkista**, **wrapkey**<br /><br />Tämä säilöön Active Directory-objektin näppäimiä käyttöoikeudet. Tämä arvo on määritettävä yksi tai useampi sallittu arvo matriisina. |
| tietoja | Matriisi<br />Pakollinen<br />**kaikki**, **Poista**, **Hae**, **luettelon**, **määrittäminen**<br /><br />Tietoja Active Directory-objektin tämän säilöön-käyttöoikeudet. Tämä arvo on määritettävä yksi tai useampi sallittu arvo matriisina. |

<a id="sku" />
### <a name="propertiessku-object"></a>Properties.SKU objekti

| Nimi | Arvo |
| ---- | ---- | 
| Nimi | Luettelo<br />Pakollinen<br />**Vakio**tai **premium** <br /><br />KeyVault käyttämään palvelutaso.  Standard tukee tietoja ja ohjelmisto suojattu avaimet.  Premium Lisää tuki:: HSM suojattu avaimet. |
| tuoteperhe | Luettelo<br />Pakollinen<br />**A** <br /><br />Tuoteperheen käyttämään. |
 
    
## <a name="examples"></a>Esimerkkejä

Seuraavassa esimerkissä ottaa käyttöön avaimen säilö ja salaisen.

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

## <a name="quickstart-templates"></a>Pikaopas-mallit

Seuraavat pikaopas malli otetaan käyttöön avaimen säilö.

- [Luo avaimen säilö](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Seuraavat vaiheet

- Yleisiä tietoja avaimen vaults artikkelissa [Azure avaimen säilö käytön aloittaminen](./key-vault/key-vault-get-started.md).
- Esimerkki viittaava avaimen säilö salaisuus otettaessa mallit-kohdassa [siirtää suojatun arvot käyttöönoton aikana](resource-manager-keyvault-parameter.md).

