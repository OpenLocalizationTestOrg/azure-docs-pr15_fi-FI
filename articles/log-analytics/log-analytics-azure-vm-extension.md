<properties
    pageTitle="Azuren näennäiskoneiden yhdistäminen Log Analytics | Microsoft Azure"
    description="Windows- ja Linux näennäiskoneiden Azure-suositeltu tapaa, jolla kerätään lokit ja arvot on asentamalla Log Analytics Azure AM-tunniste. Voit Azure portal tai PowerShellin asentaminen sivulle Azure VMs Log Analytics virtuaalikoneen-tunniste."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Yhteyden muodostaminen Log Analytics Azure-virtuaalikoneissa

Windows- ja Linux tietokoneiden suositeltava tapa kerätä lokit ja arvot on asentamalla Log Analytics-agentti.

Helpoin tapa Log Analytics-agentti asentaminen Azuren näennäiskoneiden on lokin Analytics AM-tunniste.  Tunnisteen yksinkertaistaa asentunut ja määrittää automaattisesti tietojen lähettäminen lokin Analytics-työtila, joka määritetään agentti. Agentti myös päivitetään automaattisesti, varmistetaan, että sinulla on uusimmat ominaisuudet ja korjaukset.

Windowsin näennäiskoneiden käyttöön *Microsoft Agent seuranta* virtuaalikoneen tunniste.
Linux näennäiskoneiden otat *Linux OMS agentti* virtuaalikoneen tunniste.

Lisätietoja [Azure virtuaalikoneen tunnisteet](../virtual-machines/virtual-machines-windows-extensions-features.md) ja [Linux agentti] (... / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Kun käytät agent-pohjainen sivustokokoelman lokitiedot, sinun on määritettävä [tietolähteiden Log Analytics](log-analytics-data-sources.md) lokit ja arvot, jotka haluat kerätä.

>[AZURE.IMPORTANT] Jos määrität lokin Analytics indeksi lokitiedot [Azure diagnostiikan](log-analytics-azure-storage.md)avulla ja määrität sama lokien kerääminen agentti, lokit kerätään kahdesti. Perittävän kummankin tietolähteen kohdalla. Jos sinulla on agentti asennettu, ja valitse olisi kerää tietoja käyttämällä yksin agent - loki Analytics kerätä tietoja Azure diagnostiikka ei määrittäminen.

Kolme helppoa tapaa Ota loki Analytics virtuaalikoneen tunniste on:

+ Mukaan Azure-portaalissa
+ Azure PowerShell-toiminnolla
+ Azure Resurssienhallinta-mallin avulla

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Ota käyttöön Azure-portaalissa AM-tunniste

Voit asentaa Log Analytics-agentti ja muodostaa Azure virtuaalikoneen, se suoritetaan käyttämällä [Azure portal](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Asenna Log Analytics-agentti ja virtuaalikoneen yhdistäminen Log Analytics-työtila

1.  Kirjaudu sisään [Azure portal](http://portal.azure.com).
2.  Valitse **Selaa** -portaalissa vasemmassa reunassa ja siirry **Log Analytics (OMS)** ja napsauttamalla sitä.
3.  Lokitiedoston Analytics työtilat luettelosta valitse se, jota haluat käyttää Azure AM.  
    ![OMS-työtilat](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  Valitse **analytics hallinta**-kohdassa **näennäiskoneiden**.  
    ![Näennäiskoneiden](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  Valitse **näennäiskoneiden**luettelo virtuaalikoneen, johon haluat asentaa agentti. **OMS yhteyden tila** AM tarkoittaa, että se **ei ole yhdistetty**.  
    ![AM tarkasteltaviksi](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  Virtuaalikoneen tiedot Valitse **Yhdistä**. Agentti asennetaan ja määritetään lokin Analytics-työtilan automaattisesti. Tämä prosessi kestää muutaman minuutin ajan OMS yhteyden tila on *yhteyden...*  
    ![Yhdistä AM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Kun olet asentanut ja muodostaa agentti, **OMS yhteyden** tilan päivitetään näyttämään **tämän työtilan**.  
    ![Yhdistetty](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>Ota käyttöön AM-tunniste PowerShellin avulla

Eri komentoja Azure perinteinen näennäiskoneiden ja Resurssienhallinta näennäiskoneiden on. Seuraavassa on esimerkkejä perinteinen ja Resurssienhallinta virtual koneet.

Perinteinen näennäiskoneiden Käytä seuraavaa PowerShell esimerkkiä:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Resurssienhallinta näennäiskoneiden Käytä seuraavaa PowerShell-esimerkkiä:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Kun määrität virtuaalikoneen PowerShellin avulla, sinun täytyy **Työtilan tunnus** -ja **Perusavain**. Voit etsiä tunnuksen ja avain **asetukset** -sivulla OMS-portaalin tai PowerShell-toiminnolla edellisen esimerkin mukaisesti.

![Työtilan tunnuksen ja perusavain](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Ottaa käyttöön AM-tunniste mallin avulla

Azure resurssien hallinnan avulla voit luoda yksinkertaisen mallin (JSON muodossa), joka määrittää käyttöönotto ja määritys-sovelluksen. Tämän mallin kutsutaan Resurssienhallinta-malli ja tarjoaa sääntöpohjaisesti käyttöönoton määrittämiseen. Mallin avulla voit toistuvasti kaikkialla app elinkaari sovelluksen käyttöönotto ja resurssit ovat otetaan käyttöön yhtenäinen tilaan luottaa.

Lisäämällä niihin Log Analytics-agentti Resurssienhallinta mallin, voit varmistaa, että kunkin virtuaalikoneen on valmiiksi raportin Log Analytics-työtilaan.

Saat lisätietoja Resurssienhallinta malleista [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md).

Seuraavassa on esimerkki Resurssienhallinta-malli, jota käytetään käyttöönoton virtual tietokoneeseen, jossa on käynnissä Windows asennettuna Microsoft Agent seuranta-tunniste. Tämä malli on tyypillinen virtuaalikoneen malli, seuraavat lisäysten kanssa:

+ workspaceId ja workspaceName parametrit
+ Microsoft.EnterpriseCloud.Monitoring resurssi tunniste-osa
+ Etsi workspaceId ja workspaceSharedKey tulostus


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Voit ottaa mallin käyttöön PowerShell-komennon avulla:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Windows-näennäiskoneiden vianmääritys

Jos *Microsoft Agent seuranta* AM agent-tunniste ei asentaminen tai reporting voit suorittaa ongelman vianmäärityksen seuraavasti.

1. Tarkista, onko Azure AM-agentti on asennettu ja toimimasta oikein [2965986 Knowledge Base](https://support.microsoft.com/kb/2965986#mt1)-ohjeiden mukaisesti.
  + Voit myös tarkastella AM agentti lokitiedosto`C:\WindowsAzure\logs\WaAppAgent.log`
  + Jos lokiin ei ole, AM-agentti ei ole asennettu.
    - [Perinteinen VMs Azure AM agentti asentaminen](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Vahvista Microsoft Agent seuranta tunniste uudelleenaktivoinnin yhteydessä tehtävä on käynnissä seuraavalla tavalla:
  + Kirjaudu sisään virtuaalikoneen
  + Avaa Tehtävien ajoitus ja Etsi `update_azureoperationalinsight_agent_heartbeat` tehtävä
  + Tarkista tehtävän on käytössä, ja se on käynnissä yhtä minuutin välein
  + Uudelleenaktivoinnin yhteydessä lokitiedoston sisäänkuittaus`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Tarkista Microsoft Agent-AM seuranta tunniste lokitiedostot`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Varmista virtuaalikoneen PowerShell-komentosarjojen suorittamisen
4. Varmista C:\Windows\temp käyttöoikeuksia ei ole muutettu.
5. Microsoft Agent seuranta tilan tarkasteleminen kirjoittamalla laajennettuja PowerShell-ikkunassa virtual tietokoneeseen.`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Tarkista Microsoft Agent seurantaa asennuksen lokitiedostot:`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Saat lisätietoja viitata [vianmääritys Windows-laajennuksia](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md).

## <a name="troubleshooting-linux-virtual-machines"></a>Linux näennäiskoneiden vianmääritys

Jos *OMS-agentti Linux* AM agent-tunniste ei asentaminen tai reporting voit suorittaa ongelman vianmäärityksen seuraavasti.

1. Jos laajennuksen tila on *Tuntematon* valintaruutu, jos Azure AM-agentti asennettu ja toimimasta oikein mukaan tarkistaminen AM agentti lokitiedosto`/var/log/waagent.log`
  + Jos lokiin ei ole, AM-agentti ei ole asennettu.
  - [Azure AM-agentti asentaminen Linux VMs](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Muut perusasemassa tilat Tarkista OMS-agentti varten Linux AM tunniste lokitiedostojen `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` ja`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Jos laajennuksen tila on kunnossa, mutta tiedot tuodaan ei tarkista Linux lokitiedostojen OMS-agentti`/var/opt/microsoft/omsagent/log/omsagent.log`

Saat lisätietoja viitata [vianmääritys Linux tunnisteet](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md).


## <a name="next-steps"></a>Seuraavat vaiheet

+ Määritä [lokin Analytics tietolähteiden](log-analytics-data-sources.md) lokit ja kerääminen arvot.
+ Tietojen keräämistä virtual koneet [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md).
+ Muita resursseja, joissa on Azure [kerää tietoja Azure Diagnostiikan avulla](log-analytics-azure-storage.md) .

Tietokoneissa, joissa ei ole Azure voit asentaa Log Analytics-agentti tavoilla, jotka on kuvattu seuraavissa artikkeleissa:

+ [Windows-tietokoneissa yhdistäminen Log Analytics](log-analytics-windows-agents.md)
+ [Log Analytics Linux tietokoneiden yhdistäminen](log-analytics-linux-agents.md)
