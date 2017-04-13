<properties
   pageTitle="Automaatio resurssien OMS ratkaisuissa | Microsoft Azure"
   description="Ratkaisuja OMS yleensä sisällytetään runbooks Azure automaatio automatisoida prosesseja, kuten käsitteleminen seurantatiedot.  Tässä artikkelissa käsitellään sisällyttää ratkaista runbooks ja niihin liittyvät resurssit."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automaatio resurssien OMS ratkaisuissa (ennakkoversio)

>[AZURE.NOTE]Tämä on alustava luomiseen ratkaisujen OMS, jotka ovat tällä hetkellä esikatselu. Minkä tahansa rakenteen jäljempänä sitä saatetaan muuttaa.   

[Ratkaisujen OMS-](operations-management-suite-solutions.md) yleensä sisällytetään runbooks Azure automaatio automatisoida prosesseja, kuten käsitteleminen seurantatiedot.  Lisäksi runbooks automaatio-tilien sisältää kohteita, kuten muuttujat ja aikatauluja, jotka tukevat käytetään runbooks ratkaisu.  Tässä artikkelissa käsitellään sisällyttää ratkaista runbooks ja niihin liittyvät resurssit.

>[AZURE.NOTE]Tämän artikkelin esimerkkejä käyttää parametrit ja muuttujat, jotka ovat tarvittavat tai yhteiset ratkaisujen ja kuvattu [luominen ratkaisujen toimintojen hallinta Suite (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että olet jo tuttuja siitä, miten voit [luoda management-ratkaisun](operations-management-suite-solutions-creating.md) ja ratkaisutiedoston rakenne.

## <a name="samples"></a>Esimerkkejä, joiden
Saat Resurssienhallinta esimerkkimallit Automation resurssien [GitHub pikaopas mallit](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Automaatio-tili
Azure Automation kaikki resurssit sisältyvät [automaatio-tili](../automation/automation-security-overview.md#automation-account-overview).  Kuvatulla tavalla [OMS työtilan ja automaatio tilin](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) automaatio-tili ei sisälly management-ratkaisun, mutta on oltava olemassa ennen ratkaisu on asennettu.  Jos se ei ole käytettävissä-ratkaisun asentaminen epäonnistuu.

Kirjoita automaatio kunkin resurssin nimi on niiden automaatio-tilin nimi.  Tämä tapahtuu ratkaisun **accountName** -parametrin runbookin resurssin seuraavan esimerkin mukaisesti.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Azure automaatio runbookin](../automation/automation-runbook-types.md) resurssien tyyppi on **Microsoft.Automation/automationAccounts/runbooks** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| runbookType | Määrittää: n runbookin. <br><br> Komentosarja - PowerShell-komentosarjaa <br>PowerShell - PowerShell-työnkulku <br> GraphPowerShell - graafinen PowerShell-komentosarjaa runbookin <br> GraphPowerShellWorkflow - graafinen PowerShell työnkulun runbookin   |
| logProgress | Määrittää, onko [meneillään tietueita](../automation/automation-runbook-output-and-messages.md) voidaan luoda: n runbookin. |
| logVerbose  | Määrittää, onko [yksityiskohtainen tietueita](../automation/automation-runbook-output-and-messages.md) voidaan luoda: n runbookin. |
| kuvaus | Valinnainen kuvaus: n runbookin. |
| publishContentLink | Määrittää: n runbookin sisällön. <br><br>URI - Uri: n runbookin sisällön.  Tämä on PowerShellistä ja komentosarjan runbooks .ps1 tiedoston ja Graph runbookin viedyn graafinen runbookin-tiedostoksi.  <br> versio - Version runbookin oman seurantaa varten. |

Alla on esimerkki runbookin resurssi.  Tässä tapauksessa se hakee: n runbookin [PowerShell-valikoimassa](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Aloittaminen runbookin
Voit aloittaa hallintaratkaisu, joka runbookin kahdella eri tavalla.

- Aloita: n runbookin, kun ratkaisu on asennettu, luo [työn resurssin](#automation-jobs) seuraavalla tavalla.
- Aloita: n runbookin aikataulun, luo [aikataulun resurssin](#schedules) seuraavalla tavalla. 


## <a name="automation-jobs"></a>Automaatio töitä
Jotta voit aloittaa runbookin management-ratkaisun asennuksen yhteydessä, voit luoda **projektin** resurssin.  Projektin resursseihin tyyppi on **Microsoft.Automation/automationAccounts/jobs** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| runbookin    | Yksittäisen **nimen** kohde: n runbookin nimi.  |
| Parametrit | Kohteen kullekin parametrille: n runbookin vaatii. |


Työn sisältää runbookin nimen ja: n runbookin lähettämisen parametriarvot.  Työn on [määräytyvät sen mukaan](operations-management-suite-solutions-creating.md#resources) , joka alkaa jälkeen: n runbookin runbookin on luotava ennen työtä.  Sinun on luotava muiden töissä runbooks, joka suorittaa ennen nykyisen rivin loppuun riippuvuudet.

Työn resurssin nimen on oltava GUID-tunnus, joka on yleensä määrittämän parametrin.  Voit lukea lisää tietoja GUID parametrien [luominen ratkaisuja toimintojen hallinta Suite (OMS)](operations-management-suite-solutions-creating.md#parameters).  

Seuraavassa on esimerkki työn resurssi, joka käynnistyy runbookin management-ratkaisun asennuksen yhteydessä.  Muut runbooks on suoritettava, ennen kuin tämän jokin käynnistyy, siten, että se on riippuvuuksia, runbookin töissä.  Projektin tiedot ovat käytettävissä parametrit ja muuttujien asemesta suoraan määritetty.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Varmenteet
[Azure automaatio varmenteet](../automation/automation-certificates.md) on eräänlainen **Microsoft.Automation/automationAccounts/certificates** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| base64Value   | Varmenteen Base 64 arvo. |
| allekirjoitus    | Varmenteen allekirjoitus. |

Alla on esimerkki varmenteen resurssin.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Tunnistetiedot
[Azure automaatio tunnistetiedot](../automation/automation-credentials.md) on eräänlainen **Microsoft.Automation/automationAccounts/credentials** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| Käyttäjänimi   | Tunnistetieto käyttäjänimi. |
| salasana   | Tunnistetieto salasana. |

Alla on esimerkki tunnistetiedon resurssin.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Aikatauluja
[Azure automaatio](../automation/automation-schedules.md) on eräänlainen **Microsoft.Automation/automationAccounts/schedules** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| kuvaus | Valinnainen kuvaus aikataulun. |
| Aloitusajan   | Määrittää aikataulun alkamispäivää DateTime-objekteina. Merkkijonon voidaan suorittaa, jos se voidaan muuntaa kelvollinen päivämäärä ja aika. |
| isEnabled   | Määrittää, onko aikataulun käytössä. |
| väli    | Aikataulun aikavälin tyyppi.<br><br>päivä<br>Tunti |
| taajuus   | Taajuus, aikataulun olisi Kyselysäännön päivinä tai tuntia. |

Alla on esimerkki aikataulun resurssin.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Muuttujat
[Azure automaatio muuttujat](../automation/automation-variables.md) tyyppi on **Microsoft.Automation/automationAccounts/variables** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| kuvaus | Muuttujan valinnainen kuvaus. |
| isEncrypted | Määrittää, onko muuttujan salattu. |
| tyyppi        | Muuttujan tietotyyppi. |
| arvo       | Muuttujan arvo. |

Alla on esimerkki muuttujan resurssin.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Moduulit
Management-ratkaisun ei tarvitse määrittää [Yleinen moduulit](../automation/automation-integration-modules.md) käyttämä oman runbooks, koska ne ovat aina käytettävissäsi.  Sinun täytyy lisätä muita oman runbooks käyttämä moduulin resurssin ja: n runbookin olisi määräytyvät moduulin resurssin varmistaa, että se on luotu ennen: n runbookin.

[Integrointi moduulit](../automation/automation-integration-modules.md) tyyppi on **Microsoft.Automation/automationAccounts/modules** ja sen ominaisuudet ovat seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| contentLink | Määrittää moduulin sisällön. <br><br>URI - Uri: n runbookin sisällön.  Tämä on PowerShellistä ja komentosarjan runbooks .ps1 tiedoston ja Graph runbookin viedyn graafinen runbookin-tiedostoksi.  <br> versio - Version runbookin oman seurantaa varten. |

Alla on esimerkki moduulin resurssin.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Moduulit päivittäminen
Jos päivität management-ratkaisun, joka sisältää runbookin, joka käyttää aikataulun ja ratkaisu uusi versio on kyseisen runbookin käyttämä uusi moduuli,: n runbookin voi käyttää moduulin vanhaa versiota.  Pitäisi sisältää seuraavat runbooks ratkaisu ja luo työn suorittamaan ne ennen muita runbooks.  Näin varmistat, että moduulit päivitetään kuin vaaditaan ennen runbooks ladataan.

- [Päivityksen ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) varmistat, että kaikki ratkaisu runbooks käyttämät moduulit on uusin versio.  
- [ReRegisterAutomationSchedule-MS-hallintoraportointi](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) Rekisteröi kaikki varmistaa, että runbooks linkitetty ja uusimmat moduulit aikataulu-resurssit.


Seuraavassa on esimerkki tukemaan moduulin päivitys ratkaista tarvittava elementti.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisää ratkaisu](operations-management-suite-solutions-resources-views.md) kerättyjen tietojen visualisoimiseen.