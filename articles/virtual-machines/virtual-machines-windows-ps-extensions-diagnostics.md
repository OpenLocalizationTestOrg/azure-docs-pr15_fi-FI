<properties
    pageTitle="Käyttöön Azure diagnostiikka virtual koneeseen, jossa käytetään Windows PowerShellin avulla | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Opettele käyttöön Azure diagnostiikka virtual koneeseen, jossa käytetään Windows PowerShellin avulla"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Käyttöön Azure diagnostiikka virtual koneeseen, jossa käytetään Windows PowerShellin avulla

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure Diagnostiikka on sisällä Azure, ottaa käyttöön sovelluksen diagnostiikan tiedot sivustokokoelman ominaisuuksien. Voit kerää diagnostiikkatiedot, kuten sovelluksen lokit tai suorituskyvyn laskureita Azure virtual-tietokoneesta (AM), jossa on käynnissä Windows diagnostiikka-tunniste. Tässä artikkelissa käsitellään diagnostiikka-tunniste käyttöön AM Windows PowerShellin avulla. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) varten tämän artikkelin tarvittavat yhteyden edellytykset ovat olemassa.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Ota käyttöön diagnostiikka-tunniste, jos käytät resurssien hallinnan käyttöönottomalli

Voit ottaa diagnostiikka-tunniste samalla, kun Windows-AM kautta Azure resurssien hallinnan käyttöönotto-mallin luominen lisäämällä tunniste-määritysten Resurssienhallinta-malli. Lisätietoja on kohdassa [Create virtual-Windows-tietokoneessa, jossa seuranta- ja diagnostiikka Azure Resurssienhallinta-mallin avulla](virtual-machines-windows-extensions-diagnostics-template.md).

Käyttöön aiemmin AM, joka on luotu kautta resurssien hallinnan käyttöönottomalli diagnostiikka-tunniste, voit käyttää [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) PowerShell-cmdlet-komennon alla kuvatulla tavalla.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* on polku tiedosto, joka sisältää XML-diagnostiikkamääritykset [Esimerkki](#sample-diagnostics-configuration) alla kuvatulla tavalla.  

Jos Diagnostiikka-kokoonpanotiedosto määrittää **StorageAccount** elementti tallennustilan tilin nimellä, valitse *Määrittäminen AzureRMVMDiagnosticsExtension* komentosarja määrittää automaattisesti diagnostiikan tietojen lähettäminen tallennustilan tilin diagnostiikka-tunniste. Tätä varten tallennustilan tilin on oltava sama AM-tilauksen.

Jos **StorageAccount** ei ole määritetty diagnostiikan määrityksissä, sinun on *StorageAccountName* -parametrissa siirtää-cmdlet-komennolla. Jos *StorageAccountName* -parametri on määritetty, valitse cmdlet käyttää aina-parametrissa määritetyn tallennustilan tilin ja ei esitys, joka on määritetty diagnostiikka määritystiedostossa.

Jos Diagnostiikka-tallennustilan tilin on eri tilauksen AM, sinun on eksplisiittisesti välittää *StorageAccountName* ja *StorageAccountKey* Parameters-cmdlet-komennolla. *StorageAccountKey* -parametria ei tarvita, kun diagnostiikka tallennustilan-tili on saman tilauksen-cmdlet-komennolla voit kysely ja määritä avainarvon, kun otat käyttöön diagnostiikka-tunniste. Kuitenkin Jos Diagnostiikka-tallennustilan tilin on eri tilauksen, valitse cmdlet ei välttämättä voi hankkia avain automaattisesti eikä sinun tarvitse erikseen määrittää avaimen *StorageAccountKey* -parametrin kautta.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Kun diagnostiikka-tunniste on otettu käyttöön AM, saat asetuksissa [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) cmdlet-komennolla.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Cmdlet palauttaa *PublicSettings*, joka sisältää Base64-koodattu muodossa XML-määritys. Jos haluat lukea XML-purkaa sen.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

[Poista AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) cmdlet-komennon avulla voidaan poistaa diagnostiikka-tunniste AM.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Ota käyttöön diagnostiikka-tunniste, jos käytät perinteistä käyttöönottomalli

Voit käyttää [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) cmdlet-komento käyttöön AM, jonka luot – perinteinen käyttöönottomalli on diagnostiikka-tunniste. Seuraavassa esimerkissä uuden AM – perinteinen käyttöönotto-mallin luominen käytössä diagnostiikka-tunniste.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Voit ottaa käyttöön aiemmin AM, joka on luotu – perinteinen käyttöönoton mallin diagnostiikka-tunniste, käyttää [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) cmdlet-komento ensin avulla AM-määritys. Päivitä AM määritykset sisältämään diagnostiikka-tunniste [Määrittäminen AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) cmdlet-komennolla. Käyttää päivitettyä määritys-AM käyttämällä [Päivityksen AzureVM](https://msdn.microsoft.com/library/mt589121.aspx).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Esimerkki diagnostiikka määritys

Seuraavat XML voidaan diagnostiikka julkisen määrityksen yllä komentosarjojen kanssa. Esimerkki määritysten siirtää eri suorituskyvyn laskureita diagnostiikka tallennustilan tilin, sekä sovellus, suojaus ja Windowsin tapahtumalokien järjestelmän kanavien virheitä ja diagnostiikka infrastruktuurin lokien virheitä.

Määritystä, on päivitettävä, jotta ovat seuraavat:

- **Arvot** -elementin *resourceID* -määrite on päivitetään Resurssitunnus AM.
    - Resurssitunnus on rakennettava käyttämällä seuraavaa kaavaa: "/resourceGroups//tilaukset / {*Tilaustunnus AM-tilausta*} {*resourcegroup nimi AM*} / {*AM nimi*} providers/Microsoft.Compute/virtualMachines/".
    - Esimerkiksi jos Tilaustunnus tilaukseen, jossa on käynnissä AM on **11111111-1111-1111-1111-111111111111**resurssiryhmän resurssin ryhmän nimi on **MyResourceGroup**ja AM on **MyWindowsVM**, valitse *resourceID* arvo on seuraava:

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Lisätietoja siitä, miten arvot luodaan suorituskyvyn laskureita ja arvot määritysten on artikkelissa [Azure diagnostiikka arvot taulukon tallennustilaan](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage).

- **StorageAccount** elementti on päivity diagnostiikka-tallennustilan tilin nimi.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Seuraavat vaiheet
- Katso lisäohjeita käyttämällä Azure diagnostiikka-ominaisuuksien ja muita menetelmiä ongelmien [Ottaminen käyttöön diagnostiikka Azure Cloud Services-palveluissa ja näennäiskoneiden](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Diagnostiikan käyttömahdollisuudet rakenteen](https://msdn.microsoft.com/library/azure/mt634524.aspx) kerrotaan diagnostiikka-tunniste eri XML-määrityksiä asetukset.
