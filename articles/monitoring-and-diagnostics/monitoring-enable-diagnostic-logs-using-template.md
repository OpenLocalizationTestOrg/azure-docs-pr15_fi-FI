<properties
    pageTitle="Ottaa käyttöön automaattisesti diagnostiikan asetusten Resurssienhallinta mallin avulla | Microsoft Azure"
    description="Opettele Resurssienhallinta-mallin avulla voit luoda diagnostiikan asetuksia, joiden avulla voit käyttää omaa vianmäärityslokit tapahtuman porttiin ja tallentaa ne tallennustilan tilin."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Ottaa automaattisesti käyttöön diagnostiikan asetusten resurssin luominen Resurssienhallinta mallia käyttämällä
Tässä artikkelissa on Näytä käyttämisestä [Azure Resurssienhallinta-mallin](../resource-group-authoring-templates.md) toista diagnostiikan asetusten resurssin, kun se luodaan. Näin voit käynnistää automaattisesti streaming vianmäärityslokit ja arvot tapahtuman porttiin, arkistointia tallennustilan tilillä tai lähettämällä ne Log Analytics resurssin luomisen yhteydessä.

Menetelmä ottaminen käyttöön Resurssienhallinta mallin avulla vianmäärityslokit määräytyy resurssin laji.

- **Ei laske** resurssit (esimerkiksi verkko-käyttöoikeusryhmät, logiikan sovellusten automaatio) käyttää [Diagnostiikan asetuksia, joka on kuvattu tämän artikkelin](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Laske** Resurssit (WAD/LAD-pohjainen) käyttää [WAD/LAD kokoonpanotiedosto tässä artikkelissa kuvatulla tavalla](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

Tässä artikkelissa on kerrotaan, kuinka voit määrittää diagnostiikka joko-menetelmällä.

Perusvaiheet ovat seuraavat:

1. Mallin luominen JSON-tiedostona, joka kerrotaan, miten voit luoda resurssi ja ottaa Diagnostiikka.
2. [Ota käyttöönoton menetelmällä malli](../resource-group-template-deploy.md).

Alla on antaa sinun on luotava Laske ja Laske resurssien mallitiedoston JSON esimerkki.

## <a name="non-compute-resource-template"></a>Ei laske resurssin mallia
Laske resurssien on tehtävä kaksi asiaa:

1. Parametrit-blob tallennustilan tilin nimi, palvelun bus Sääntötunnus ja/tai OMS Log Analytics työtilan tunnus (käyttöönotto arkistointia vianmäärityslokit ja tallennustilaa tilillä, streaming lokiin tapahtuman porttiin ja/tai lähettää lokit Log Analytics) voit lisätä parametreja.

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Lisää resursseja, jonka haluat ottaa käyttöön vianmäärityslokit resurssin matriisin resurssin lajin `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Diagnostiikan asetukselle ominaisuudet-blob noudattaa [muoto, joka on kuvattu tämän artikkelin](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Seuraavassa on koko esimerkki, joka luo verkon käyttöoikeusryhmän ja ottaa käyttöön toistamisen tapahtuman keskittimet ja tallennustilaa tallennustilan tilillä.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Laske resurssin mallia
Laske resurssin diagnostiikka käyttöön esimerkiksi virtuaalikoneen tai palvelun kangasta-klusteriin on:

1. Lisää AM resurssin määritelmän Azure diagnostiikka-tunniste.
2. Määritä tallennustilan tilin ja/tai tapahtumaa-toiminnossa parametrina.
3. Lisää WADCfg XML-tiedoston sisällön XMLCfg ominaisuuteen vuotamaan kaikki XML-merkit oikein.

> [AZURE.WARNING] Tämä viimeinen vaihe voi olla vaikeaa oikea. [Katso tämän artikkelin](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) esimerkki, joka jakaa diagnostiikka määritysten rakenteen kyselyjä muuttujat, jotka ovat ohitettuja ja muotoiltu oikein.

Koko prosessin, malleja, kuten on kuvattu [tämän asiakirjan](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja on artikkelissa Azure vianmäärityslokit](./monitoring-overview-of-diagnostic-logs.md)
- [Virtauttaa Azure vianmäärityslokit tapahtuman porttiin](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
