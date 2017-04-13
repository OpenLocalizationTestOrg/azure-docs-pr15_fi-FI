<properties
    pageTitle="Luo ja määritä lokin Analytics työtilan PowerShellin avulla | Microsoft Azure"
    description="Kirjaudu Analytics käyttää tietojen tiloissa käyttäjän palvelimelta tai cloud infrastruktuuria. Tietokoneen tietojen kerääminen Azure säilöstä, kun luoma Azure Diagnostiikka."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Hallitse Log Analytics PowerShellin avulla

Voit käyttää [Log Analytics PowerShellin cmdlet-komennot] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) suorittamaan erilaisia toimintoja Log Analytics komentoriviltä tai komentosarjan osana.  Esimerkkejä PowerShellin avulla voit suorittaa tehtäviä:

+ Työtilan luominen
+ Lisää tai Poista ratkaisu
+ Tuominen ja vieminen tallennetut haut
+ Tietokoneen ryhmän luominen
+ Ota käyttöön kokoelma IIS-lokeja tietokoneista asennettu Windows-agentti
+ Suorituskyvyn laskureita kerätään Linux ja Windows-tietokoneissa
+ Tapahtumien kerätään syslog Linux tietokoneissa 
+ Tapahtumien kerääminen Windowsin tapahtumalokien
+ Mukautetun tapahtumalokien kerääminen
+ Lisää Azure virtuaalikoneen log analytics-agentti
+ Määritä lokin analytics indeksi tietojen kerääminen Azure Diagnostiikan avulla


Tässä artikkelissa on kaksi MALLIKOODEJA, jotka kuvaavat jotkin toiminnot, jotka voit tehdä PowerShell.  Voit katsoa [Log Analytics PowerShell cmdlet-komento viittaus] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) muita toimintoja varten.

> [AZURE.NOTE] Lokitiedoston Analytics aiemmin kutsuttiin toiminnallisia oivalluksia, joka on miksi se on nimi, jota käytetään Cmdlet-komentoja.

## <a name="prerequisites"></a>Edellytykset

Jos haluat käyttää PowerShell Log Analytics-työtila, sinulla on oltava:

+ Azure-tilaukseen ja 
+ Azure Log Analytics-työtilan linkitetty Azure-tilaukseen.

Jos olet luonut OMS-työtila, mutta se ei ole vielä linkitetty se Azure tilaukseen voit luoda linkin:

+ Azure-portaalissa
+ OMS portaalissa tai 
+ Voit käyttää Get-AzureRmOperationalInsightsLinkTargets ja uusi AzureRmOperationalInsightsWorkspace Cmdlet-komentoja.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Luo ja määritä lokin Analytics-työtila

Komentosarjan seuraavassa esimerkissä kuvataan, miten voit:

1.  Työtilan luominen
2.  Luettelo käytettävissä olevat ratkaisut
3.  Ratkaisujen lisääminen työtilaan
4.  Tuo tallennetut haut
5.  Vie tallennettu haut
6.  Tietokoneen ryhmän luominen
7.  Ota käyttöön kokoelma IIS-lokeja tietokoneista asennettu Windows-agentti
8.  Loogisen levyn teholaskureita kerätään Linux tietokoneiden (% käytetään Inodes Vapaa megatavua; % Käyttää tilaa; Levyn siirrot/s; Levyn lukuja/s; Levyn kirjoituksia/s)
9.  Kerätä syslog tapahtumia Linux tietokoneista
10. Kerää virhe ja varoitus tapahtumat-sovelluksen tapahtumalokiin Windows-tietokoneissa
11. Megatavuja käytettävissä oleva muisti suorituskyvyn laskuri kerätään Windows-tietokoneissa
12. Mukautetun lokitiedoston kerääminen 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Log Analytics indeksoida Azure diagnostiikan asetusten määrittäminen 

Resurssien Azure resurssien agentitonta seuranta, on on otettu käyttöön ja määrittänyt kirjoittaa tallennustilan tilin Azure Diagnostiikka. Lokitiedoston Analytics voidaan määrittää kerätä lokit tallennustilan-tililtä. Resurssit, jotka haluat tehdä edellisen määritys sisältyy:

+ Perinteinen pilvipalveluihin (Internetin kautta tai työntekijä roolit)
+ Palvelun kangasta klustereiden
+ Verkon käyttöoikeusryhmät
+ Avaimen vaults ja 
+ Sovelluksen yhdyskäytävät

Voit käyttää myös PowerShell Log Analytics työtilan paikallaan yhden Azure-tilauksen lokien kerääminen eri Azure-tilauksissa.

Seuraavassa esimerkissä Toimintaohje:

1.  Olemassa olevan tallennustilan tilit ja Log Analytics indeksoi tietojen sijaintien luettelon
2.  Luo lukemaan tallennustilan tilin asetukset
3.  Uusien määritysten päivittämään indeksitiedot muihin sijainteihin
4.  Poista juuri luomasi määritys

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Seuraavat vaiheet

- [Tarkista loki Analytics PowerShellin cmdlet-komennot](http://msdn.microsoft.com/library/mt188224.aspx) Log Analytics kokoonpanon PowerShellin avulla saat lisätietoja.

