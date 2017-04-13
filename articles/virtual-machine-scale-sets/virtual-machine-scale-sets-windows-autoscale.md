<properties
    pageTitle="Automaattinen skaalaus Windows virtuaalikoneen asteikko määrittää | Microsoft Azure"
    description="Määritä automaattisen skaalauksen poistaminen Windows virtuaalikoneen asteikko määrittäminen joukolle Azure PowerShellin avulla"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Skaalaa automaattisesti virtuaalikoneen asteikko joukon koneet

Virtuaalikoneen asteikko joukot helposti käyttöön ja hallita samanlaiset näennäiskoneiden joukkona. Asteikko määrittää on erittäin skaalattava ja mukautettavissa Laske kerroksen hyperscale sovellusten ja niiden tukemista Windows platform kuvia, Linux ympäristö kuvia, mukautetut kuvia ja laajennukset. Saat lisätietoja asteikko joukot [Virtuaalikoneen asteikko määrittää](virtual-machine-scale-sets-overview.md).

Tässä opetusohjelmassa avulla voit luoda Windows näennäiskoneiden asteikko ja Skaalaa automaattisesti joukon koneet. Voit luoda asteikon määrittäminen ja skaalauskertoimen Azure Resurssienhallinta-mallin luominen ja käyttöönotto käyttämällä PowerShellin Azure määrittäminen. Saat lisätietoja malleista [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md). Lisätietoja asteikko joukot Automaattinen skaalaus-kohdassa [Automaattinen skaalaus ja virtuaalikoneen asteikko asettaa](virtual-machine-scale-sets-autoscale-overview.md).

Tässä artikkelissa käyttöönottoa seuraavia resursseja ja laajennukset:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Saat lisätietoja Resurssienhallinta resurssien [Azure Laske, verkko- ja tallennustilaa tarjoajat Azure resurssien hallinta-kohdassa](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Vaihe 1: Azure PowerShellin asentaminen

Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja PowerShellin Azure uusimman version asentamisesta, valitsemalla tilauksen ja Azure kirjautuminen.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Vaihe 2: Luo resurssiryhmä ja tallennustilaa tilin

1. **Luo resurssiryhmä** – kaikki resurssit on otettava käyttöön resurssiryhmä. [Uusi AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) avulla voit luoda resurssiryhmän nimeltä **vmsstestrg1**.

2. **Luo tallennustilan tili** – tallennustilan tilin on, jossa malli on tallennettu. [Uusi AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) avulla voit luoda nimeltä **vmsstestsa**tallennustilan tilin.

## <a name="step-3-create-the-template"></a>Vaihe 3: Mallin luominen
Azure Resurssienhallinta-malli mahdollistaa sellaisten ottaminen käyttöön ja hallita Azure resursseja yhdessä JSON kuvaus resurssit ja liittyvän käyttöönoton parametrien avulla.

1. Suosikit-editorissa C:\VMSSTemplate.json-tiedoston luominen ja lisää alkuperäisen JSON rakenne, tukemaan malli.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parametrit eivät ole aina pakollisia, mutta niissä voi kirjoittaa arvoja, kun malli otetaan käyttöön. Lisää nämä parametrit parametrit ylemmän tason elementti, jonka lisäsit malli-kohdassa.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Määrittää erillisen virtuaalikoneen, jota käytetään käyttää asteikon koneet nimi.
    - Jos malli on tallennettu tallennustilan tilin nimi.
    - Luo aluksi asteikon näennäiskoneiden määrän määrittäminen.
    - Nimi ja näennäiskoneiden järjestelmänvalvojatilin salasana.
    - Määritä etuliite resursseille, jotka on luotu tukemaan asteikko.

3. Muuttujia voi käyttää mallissa, voit määrittää arvot, jotka saattavat muuttua usein tai arvoja, jotka on luotu parametriarvojen yhdistelmän on. Lisää muuttujia muuttujat ylemmän tason elementti, jonka lisäsit malli-kohdassa.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - DNS-nimiä, jotka käyttävät Verkkoliittymät.
    - IP-osoite nimien ja etuliitteiden VPN-ja aliverkosta.
    - Nimet ja tunnisteet virtual verkoston ladata tasaustoiminto ja verkko-rajapintojen.
    - Tallennustilan tilien nimet asteikon koneet liittyvät tilien määrittäminen.
    - Diagnostiikka-tunniste, joka on asennettu näennäiskoneiden asetukset. Lisätietoja diagnostiikka-tunniste on artikkelissa [luominen Windowsin Virtual tietokoneessa, jossa seuranta- ja diagnostiikka Azure Resurssienhallinta mallin avulla](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Lisää resursseja ylemmän tason elementti, jonka lisäsit mallin tallennustilan tilin resurssiin. Tätä mallia käytetään luomaan suositellut viisi tallennustilan tilit silmukan käyttöjärjestelmän levyjen ja diagnostiikkatiedot tallennuspaikkaa. Tämän kohdan arvoksi tilien tukee enintään 100 näennäiskoneiden asteikko joukon, eli nykyisen suurin. Kunkin tallennustilan tilin nimi on kirjain vuorokaudenajan ilmaisin, joka on määritetty yhdistettynä etuliite, joka saadaan Parametrit mallin muuttujat.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Lisää VPN-resurssi. Lisätietoja on artikkelissa [Resurssin verkkopalvelun](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Lisää julkisen IP-osoite resursseja, joita käytetään kuormituksen ja verkkoliittymän.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. Lisää kuormituksen tasauspalvelun resurssi, jota käytetään asteikko-määrittäminen. Lisätietoja on artikkelissa [Azure Resurssienhallinta tuki kuormituksen](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Lisää verkko-käyttöliittymän resurssi, jota käytetään eri virtuaalikoneen. Koska koneet asteikko joukon ole julkisen IP-osoitteen kautta, erillisessä virtual machine luodaan samassa virtual verkossa etäyhteyden Access koneet.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Lisää erillinen virtuaalikoneen asteikko joukkona samassa verkossa.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
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
            }
          }
        },

10. Lisää virtuaalikoneen Skaalaa määrittäminen resurssien ja määritä diagnostiikka-tunniste, joka on asennettu kaikki näennäiskoneiden asteikko-määrittäminen. Monet asetukset tälle resurssille muistuttavat virtuaalikoneen resurssi. Tärkein ero on kapasiteetin elementti, joka määrittää, kuinka monta näennäiskoneiden asteikko määrittäminen ja upgradePolicy, joka määrittää, miten päivitykset tehdään näennäiskoneiden. Skaalaus-määrittäminen ei ole luotu, kunnes kaikki tallennustilan tilit on luotu määritysten mukaisesti dependsOn elementti.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
                  "extensionProfile": {
                    "extensions": [
                      {
                        "name": "Microsoft.Insights.VMDiagnosticsSettings",
                        "properties": {
                          "publisher": "Microsoft.Azure.Diagnostics",
                          "type": "IaaSDiagnostics",
                          "typeHandlerVersion": "1.5",
                          "autoUpgradeMinorVersion": true,
                          "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Lisää autoscaleSettings resurssi, joka määrittää, miten asteikko-määrittäminen säätää suorittimen käyttö tietokoneissa asteikko-määrittäminen perustuu.

            {
              "type": "Microsoft.Insights/autoscaleSettings",
              "apiVersion": "2015-04-01",
              "name": "[concat(parameters('resourcePrefix'),'as1')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              ],
              "properties": {
                "enabled": true,
                "name": "[concat(parameters('resourcePrefix'),'as1')]",
                "profiles": [
                  {
                    "name": "Profile1",
                    "capacity": {
                      "minimum": "1",
                      "maximum": "10",
                      "default": "1"
                    },
                    "rules": [
                      {
                        "metricTrigger": {
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Tässä opetusohjelmassa nämä arvot ovat tärkeitä:

    - **metricName** – tämä arvo on sama kuin suorituskyvyn laskuri, joka on määritetty wadperfcounter-muuttuja. Käytä kyseistä muuttujaa, diagnostiikka-tunniste kerää **suoritin(_Yhteensä)\% ajan** laskuri.
    - **metricResourceUri** - arvoksi on virtuaalikoneen Skaalaa määrittäminen resurssitunnus.
    - **timeGrain** – tämä arvo on tietoja, joista kerätään rakeisuuden. Tätä mallia se on määritetty minuutin kuluttua.
    - **Tilasto** – tämä arvo määrittää, miten määritetty yhdistetään sopimaan automaattisen skaalauksen toiminnon. Mahdolliset arvot ovat: keskiarvo, Min, Max. Tämän mallin kerätään näennäiskoneiden keskimääräinen yhteensä suorittimen käyttö.
    - **timeWindow** – tämä arvo on solualue, johon esiintymätiedot kerätään aika. Se on oltava 5 minuuttia-12 tuntia.
    - **timeAggregation** – tämä arvo määrittää, kuinka tiedot, jotka kerätään voidaan yhdistää ajan kuluessa. Oletusarvo on keskiarvon. Mahdolliset arvot ovat: keskiarvo, vähintään, enintään, viimeksi, summa, Laske.
    - **operaattori** – tämä arvo on operaattori, jota käytetään, kun haluat verrata metrisillä tiedot ja raja-arvon. Mahdolliset arvot ovat: vastaa arvoa, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **raja-arvon** – tämä arvo on arvo, joka käynnistää asteikko-toiminnon. Tämän mallin koneet lisätään mittakaava, kun keskimääräinen suorittimen käyttö, koneet, valitse Määritä välillä on yli 50 prosenttia.
    - **suunta** – tämä arvo määrittää toiminnon, joka suoritetaan, kun raja-arvo on saavutettu. Mahdolliset arvot ovat Suurenna tai Pienennä. Tämän mallin näennäiskoneiden asteikko-määrittäminen määrä kasvaa Jos raja-arvo on yli 50 prosenttia määritetyt aika-ikkunassa.
    - **Kirjoita** – tämä arvo on toiminto, joka suoritetaan, ja määritettävä ChangeCount tyyppi.
    - **arvo** , tämä arvo on näennäiskoneiden, joka lisätään tai poistetaan asteikko joukosta määrä. Tämän arvon on oltava 1 tai suurempi. Oletusarvo on 1. Tämän mallin asteikon koneet määrän määrittäminen kasvaa 1, raja-arvon täyttyessä.
    - **cooldown** – tämä arvo on aika odottamaan skaalauksen edellisen toiminnon jälkeen ennen seuraavan toiminnon. Tämän arvon on oltava minuutin ja viikkoa.

12. Tallenna mallitiedosto.    

## <a name="step-4-upload-the-template-to-storage"></a>Vaihe 4: Lataa malli tallennustila

Mallin voi ladata, kun tiedät, että nimi ja perusavain-tallennustilan tilin, jonka loit vaiheessa 1.

1.  Määritä Microsoft Azure PowerShell-ikkunan muuttuja, joka määrittää vaiheessa 1 luomasi tallennustilan tilin nimi.

            $storageAccountName = "vmstestsa"

2.  Määritä muuttuja, joka määrittää perusavaimen tallennustilan tilin.

            $storageAccountKey = "<primary-account-key>"

    Saat avaimeen napsauttamalla avaimen kuvaketta, kun tarkastelet tallennustilan tilin resurssin Azure-portaalissa.

3.  Luo tallennustilan tilin kontekstissa objekti, jota käytetään Vahvista toimintojen tallennustilan-tilillä.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Luo malli säilö.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  Lataa mallitiedoston uuteen säilöön.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Vaihe 5: Käyttöönotto malli

Nyt kun olet luonut mallin, voit aloittaa käyttöönotto resurssit. Tällä komennolla Aloita:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Kun painat enter-sinua kehotetaan antamaan sinulle määritetty muuttujan arvot. Anna seuraavat arvot:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Olisi kestää noin 15 minuuttia resurssien onnistuneesti käyttöön.

>[AZURE.NOTE] Voit käyttää myös portaalin voi ottaa käyttöön resurssit. Käytä tätä linkkiä: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Vaihe 6: Näytön resurssit

Voit hankkia tietoja virtuaalikoneen asteikko sarjat seuraavista tavoista:

 - Azure - portaalin tällä hetkellä saat rajoitetun tietojen portaalissa.
 - [Azure resurssin Explorer](https://resources.azure.com/) - tämä työkalu on paras tutustuminen asteikko määrittäminen nykyisen tilan. Toimi Tämä polku ja mittakaava joukko, jonka loit esiintymän näkymän tulee näkyviin:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell – Käytä tätä komentoa tietoja:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Yhteyden muodostaminen erillisessä virtuaalikoneen samalla tavalla kuin toiseen koneeseen ja sitten käyttöoikeus etäyhteyden näennäiskoneiden määrittäminen seurannassa yksittäisten prosessien mittakaava.

>[AZURE.NOTE] Täydellisen REST API saamiseksi asteikko joukot tietoja löytyvät [Virtuaalikoneen asteikko määrittää](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Vaihe 7: Poista resurssit

Perittävän Azure käytettävien resurssien, koska se on aina hyvä Poista resursseja, joita ei enää tarvita. Ei tarvitse poistaa kullekin resurssille erikseen resurssiryhmä. Voit poistaa resurssiryhmän ja sen resursseja poistetaan automaattisesti.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Jos haluat säilyttää resurssiryhmän, voit poistaa vain asteikko.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Seuraavat vaiheet

- Hallita mittakaava, että juuri luomasi tietojen avulla [hallita näennäiskoneiden virtuaalikoneen-asteikko-joukko](virtual-machine-scale-sets-windows-manage.md).
- Lue lisää [Pystysuuntainen Automaattinen skaalaus virtuaalikoneen asteikko joukot ja](virtual-machine-scale-sets-vertical-scale-reprovision.md) tarkastelemalla skaalauksen pystysuunnassa
- Etsi esimerkkejä seuranta ominaisuuksia [Azure näytön PowerShell nopeasti](../monitoring-and-diagnostics/insights-powershell-samples.md) aloitusnäytössä näytteiden Azure-näyttö
- Lue [käyttää Automaattinen skaalaus-toimintoja, sähköpostin ja webhook Azure näytön ilmoituksen ilmoitukset lähetetään](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md) ilmoitus ominaisuuksista
- Lue, miten [käytön valvonta kirjaa Lähetä sähköposti- ja webhook Azure näytön ilmoitusten ilmoituksia](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
