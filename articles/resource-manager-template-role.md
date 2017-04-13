<properties
   pageTitle="Resurssienhallinta mallin roolimäärityksiä | Microsoft Azure"
   description="Näyttää Resurssienhallinta rakenteen käyttöönotto roolimääritys mallin avulla."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Rooli varausten mallin rakenne

Määrittää käyttäjän, ryhmän tai palvelun lyhennys roolin määritetyllä alueella.

## <a name="resource-format"></a>Resurssimuoto

Voit luoda roolin varauksen Lisää seuraavan rakenteen mallin resurssit-osa.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Arvot

Seuraavissa taulukoissa kuvataan arvot, jotka on määritelty määrittämiseen.

| Nimi | Pakollinen | Kuvaus |
| ---- | -------- | ----------- |
| tyyppi | Kyllä    | Resurssin laji luomiseen.<br /><br /> Resurssiryhmän:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Resurssin:<br />**{nimitilan tarjoaja} / {resurssityyppi} / tarjoajien/roleAssignments** |
| apiVersion |Kyllä | API-versio, jonka avulla voi luoda resurssin.<br /><br /> Käytä **2015-07-01**. | 
| Nimi | Kyllä | Uusi roolimääritys yleisesti yksilöllinen tunnus. |
| dependsOn | Ei | CSV-matriisin resurssin nimiä tai resurssin yksilöivää tunnistetta.<br /><br />Resurssien tämän roolimääritys määräytyy sivustokokoelman. Jos roolin, joka on kohdistettu resurssille ja että resurssi on otettu käyttöön samaan malliin, Sisällytä tämä elementti, jotta resurssi on otettu käyttöön ensin resurssinimi. |
| Ominaisuudet: | Kyllä | Ominaisuudet-objekti, joka yksilöi roolimääritys, lyhennys ja laajuus. |

### <a name="properties-object"></a>objektin ominaisuudet

| Nimi | Pakollinen | Kuvaus |
| ---- | -------- | ----------- |
| roleDefinitionId | Kyllä |  Olemassa olevan roolimäärityksen, jota käytetään roolimääritys tunnus.<br /><br /> Käytä seuraava kaava:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Kyllä | Olemassa olevan lyhennys yksilöivä tunnus. Tämä arvo vastaa sisällä hakemiston tunnuksen ja voit osoittaa käyttäjän, palvelun lyhennys tai käyttöoikeusryhmän. |
| laajuus | Ei | Alue, jolla tämän roolimääritys koskee.<br /><br />Resurssin ryhmien käyttäminen:<br />**/resourceGroups/ /subscriptions/ {Tilaustunnus} {resurssi-ryhmän nimi}**  <br /><br />Resurssien käytön:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Opi käyttämään roolin varauksen resurssi

Lisää mallin roolimääritys, kun haluat lisätä käyttäjän, ryhmän tai palvelun pääasiallista rooli käyttöönoton aikana. Roolimäärityksiä periytyvät korkeammat tasot laajuuden, joten jos olet jo lisännyt lyhennys roolin tilauksen tasolla, sinun ei tarvitse määrittää resurssiryhmä tai resurssi.

Sinun on annettava, kun käsittelet roolimäärityksiä tunnus arvojen on. Voit hakea arvoja PowerShell tai Azure CLI kautta.

### <a name="powershell"></a>PowerShellin

Roolimääritys nimi edellyttää GUID-tunnus. Voit luoda uuden tunnuksen **nimen** kanssa:

    $name = [System.Guid]::NewGuid().toString()

Voit hakea roolimääritys, jonka tunniste:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Voit hakea tunnus lyhennys jollakin seuraavista komennoista.

Ryhmän nimeltä **tarkastajille**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Käyttäjän nimeltä **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Palvelun lyhennys nimettyjen **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure CLI

Voit hakea roolimääritys, jonka tunniste:

    azure role show Reader --json | jq .[].Id -r

Voit hakea tunnus lyhennys jollakin seuraavista komennoista.

Ryhmän nimeltä **tarkastajille**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Käyttäjän nimeltä **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Palvelun lyhennys nimeltä **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Esimerkkejä

Seuraavaan malliin saa tunniste rooli ja käyttäjän, ryhmän tai palvelun lyhennys tunniste. Määrittää roolin resurssin ryhmän tasolla.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Seuraava malli luo tallennustilan tilin ja määrittää lukija storage-tilille. Kaksi ryhmiä ja Lukija tunnukset on sisällytetty mallin, joka helpottaa käyttöönotto. Nämä arvot voi hakea komentosarjan kautta käyttöönoton aikaa ja parametreiksi välitetty.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Pikaopas-mallit

Seuraavat mallit Näytä käyttämisestä roolin varauksen resurssi:

- [Resurssiryhmä valmiin roolin määrittäminen](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Määrittää olemassa olevan AM valmiita rooleja](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Useita aiemmin VMs valmiin roolin määrittäminen](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja mallin rakenne on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md).
- Saat lisätietoja Roolipohjainen käyttöoikeuksien valvonta [Azure Active Directory Roolipohjainen käyttöoikeuksien valvonta](./active-directory/role-based-access-control-configure.md).
