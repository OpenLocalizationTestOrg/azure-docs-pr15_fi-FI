<properties
   pageTitle="Käyttämällä toivottuja virtuaalikoneen asteikko joukot State kokoonpanon | Microsoft Azure"
   description="Virtuaalikoneen asteikolla asettaa Azure DSC-tunniste"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Virtuaalikoneen asteikolla asettaa Azure DSC-tunniste

[Azure toivottuja tilan määrittäminen (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) tunniste käsittelijä voidaan käyttää [Virtuaalikoneen asteikko joukot (VMSS)](virtual-machine-scale-sets-overview.md) . VMSS avulla on helppo käyttöönotto ja hallitse useita näennäiskoneiden ja elastically voi skaalata sisään ja ulos lataamaan vastaus. DSC käytetään määrittäminen VMs siinä muodossa kuin ne tulee online-tilaan, jotta ne ovat käytössä tuotannon-ohjelmisto.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Erot AM ja VMSS käyttöönotto

VMSS perusrakenteen mallin poikkeaa hieman yksittäisen AM. Tarkemmin sanottuna yksittäisen AM ottaa käyttöön "virtualMachines" solmun tunnisteet. Ei merkinnän tyyppi-laajennukset", jossa DSC lisätään malli

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

VMSS solmu on "ominaisuudet-osan"VirtualMachineProfile","extensionProfile"-määrite. DSC lisätään kohtaan "laajennukset"

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>VMSS toiminta

VMSS toiminta on sama kuin yhden AM toiminnan. Kun uusi AM luodaan, automaattisesti valmisteltu DSC-tunniste. Jos uudempi versio WMF tarvitaan tunnisteen mukaan, AM käynnistetään uudelleen ennen tulossa online. On online-tilassa, kun se lataa DSC määritysten .zip ja valmistella käyttöön AM. Lisätietoja löytyy [Azure DSC tunniste](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md)yhteenvedossa.

## <a name="next-steps"></a>Seuraavat vaiheet ##
Tarkastele [Azure Resurssienhallinta mallia DSC-tunniste](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Lue, miten [DSC tunniste käsittelee suojatusti tunnistetiedot](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Lisätietoja Azure DSC tunniste käsittelijä on artikkelissa [Johdanto Azure toivottuja tilan määritys tunniste käsittelyn](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

Lisätietoja PowerShell-DSC [PowerShell dokumentaatio Centeristä](https://msdn.microsoft.com/powershell/dsc/overview). 


