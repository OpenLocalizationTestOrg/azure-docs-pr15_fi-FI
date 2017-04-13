<properties
    pageTitle="Luo Windows Virtual machine seuranta- ja diagnostiikka Azure Resurssienhallinta mallin avulla | Microsoft Azure"
    description="Azure resurssien hallinnan mallin avulla voit luoda uuden Windows virtual koneen Azure diagnostiikka-tunnisteella."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Luo Windows Virtual machine seuranta- ja diagnostiikka Azure Resurssienhallinta mallin avulla

Azure diagnostiikka-tunniste on seuranta ja diagnostiikka ominaisuuksia Windows Azure virtuaalikoneen perusteella. Voit ottaa näiden ominaisuuksien käyttöön virtuaalikoneen sisällyttämällä tunnisteen osana azure resurssien hallinnan mallia. Näet [Yhtä aikaa muiden kanssa Azure Resurssienhallinta mallit AM tunniste on](virtual-machines-windows-extensions-authoring-templates.md) lisätietoja tunnistetta niihin virtuaalikoneen mallina. Tässä artikkelissa kuvataan, kuinka voit lisätä mallille virtuaalikoneen windows Azure diagnostiikka-tunniste.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Lisää Azure diagnostiikka-tunniste AM resurssin määritys 

Käyttöön diagnostiikka-tunniste Windows Virtual Machine haluat lisätä tunnisteen hallinta mallin AM resurssiksi.

Yksinkertainen Resurssienhallinta, perustuvat virtuaalikoneen lisätä tunniste-määritysten *resurssien* matriisia virtuaalikoneen seuraavasti: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Toinen tavallinen käytäntö on tunniste-määritysten lisääminen sijaan määrittäminen virtual machine resurssien solmun mallin pääkansion resurssit-solmu. Tämän menetelmän kanssa on määritettävä erikseen hierarkkinen suhde tunniste ja virtuaalikoneen *nimi* ja *laji* -arvoilla. Esimerkki: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Tiedostotunniste on aina liitetty virtuaalikoneen, voit joko suoraan määrittää virtual machine resurssin solmun suoraan tai määrittää base tasolla ja liittäminen virtuaalikoneen hierarkkisia nimeämiskäytäntöä avulla.

Virtuaalikoneen asteikko joukkojen tunnisteet-määritys on määritetty *VirtualMachineProfile* *extensionProfile* -ominaisuudessa.
   
*Publisher* -ominaisuuden arvon **Microsoft.Azure.Diagnostics** ja **IaaSDiagnostics** arvon *tyyppi* -ominaisuuden yksilöivät Azure diagnostiikka-tunniste.

*Nimi* -ominaisuuden avulla voidaan resurssiryhmän-laajennus. Ominaisuuden arvoksi erityisesti **Microsoft.Insights.VMDiagnosticsSettings** käyttöön sen avulla on helppo tunnistaa varmistamalla Azure perinteinen portaalin portaalin, että seurantaa kaaviot näy oikein Azure perinteinen portaalin.

*TypeHandlerVersion* määrittää version haluat käyttää tunniste. Määrittäminen *autoUpgradeMinorVersion* aliversio **Tosi** varmistaa, että saat uusimmat aliversio tunniste, joka on käytettävissä. On suositeltavaa, että määrität aina *autoUpgradeMinorVersion* voi olla **Tosi** aina niin, että saat aina uusimmat käytettävissä diagnostiikka-tunniste käyttäminen uusista ominaisuuksista ja virheenkorjauksia. 

*Asetukset* -osa sisältää käyttömahdollisuudet ominaisuuksien tunniste, joka määrittää ja lukea takaisin tunniste (joskus kutsutaan julkisen määritys). *Xmlcfg* -ominaisuus sisältää xml-pohjainen määritysten diagnostiikka-lokit suorituskyvyn laskureita, kerätään diagnostiikka-agentti yms. Katso lisätietoja itse xml-rakenteen [Diagnostiikka määritysten rakenne](https://msdn.microsoft.com/library/azure/dn782207.aspx) . Yleinen käytäntö on todellinen xml-määritysten muuttujana Azure Resurssienhallinta-mallin ja sitten yhdistää ja base64 koodata ne asetetaan *xmlcfg*arvo. Katso lisätietoja tallentaminen xml-muuttujat [Diagnostiikka konfiguroinnin muuttujat](#diagnostics-configuration-variables) . *StorageAccount* -ominaisuus määrittää, johon diagnostiikka tiedot siirtyvät tallennustilan tilin nimi. 
 
*ProtectedSettings* (joskus kutsutaan yksityinen määritys) ominaisuudet voidaan määrittää, mutta se ei voi lukea takaisin määritettävään jälkeen. *ProtectedSettings* vain kirjoitus laatu on hyödyllisiä tietoja, kuten tallennustilan tilin avain tallennettaessa mihin diagnostiikka tiedot kirjoittaa.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Parametreiksi diagnostiikka-tallennustilan tilin määrittäminen 

-Diagnostiikka tunniste json koodikatkelman yllä olettaa kaksi parametrit *existingdiagnosticsStorageAccountName* ja *existingdiagnosticsStorageResourceGroup* Määritä diagnostiikka tallennustilan-tili, johon diagnostiikka-tiedot tallennetaan. Diagnostiikka-tallennustilan tilin määrittäminen, kun parametri on helppo vaihtaa diagnostiikka-tallennustilan tilin eri ympäristöissä eri esimerkiksi haluat ehkä käyttää eri diagnostiikka-tallennustilan tilin ja jonkin muun tuotannon käyttöönottoa varten.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Parhaiden käytäntöjen mukaisesti kannattaa määrittää diagnostiikka-tallennustilan tilin eri resurssiryhmä kuin virtuaalikoneen resurssiryhmän on. Resurssiryhmä voit pidetään käyttöönoton yksikkönä, jossa on oma elinaika-virtual machine käyttöön- ja myymälät uudet määritykset päivitykset on tehty sen, mutta voit halutessasi jatkaa diagnostiikka-tietojen tallentaminen saman tallennustilan tilin näiden virtuaalikoneen ominaisuuksissa yli. Tallennustilan tilin tallentamisessa eri resurssien avulla tallennustilan-tilin Hyväksy tiedot eri virtuaalikoneen ominaisuuksissa helpottaa vianmääritys eri versioiden välillä.

>[AZURE.NOTE] Jos windows virtuaalikoneen mallin luominen Visual Studio tallennustilan oletustilin voi määrittää käyttämään saman tallennustilan tilin, jossa virtuaalikoneen Näennäiskiintolevyn ladataan. Tämä on yksinkertaista AM ensimmäisen määrityskerran. Olisi uudelleen huomioon eri tallennustilan-tili, jolla voidaan välittää parametrina käytettävä malli. 

## <a name="diagnostics-configuration-variables"></a>Diagnostiikan konfiguroinnin muuttujat
 
-Diagnostiikka tunniste json koodikatkelman yllä määrittää *accountid* -muuttujaa voidaan yksinkertaistaa käytön diagnostiikka-tallennustilan tallennustilan tilin näppäintä:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Diagnostiikka-tunniste *xmlcfg* -ominaisuus on määritetty käyttämällä useita muuttujia, jotka ovat yhdistetään yhteen. Muuttujia arvot ovat xml, joten he tarvitsevat ohitettuja oikein json muuttujat määritettäessä.

Seuraavassa kuvataan diagnostiikka määritysten xml, johon kerätään vakio järjestelmän tason suorituskyvyn laskureita sekä joitakin Windowsin tapahtumalokien ja diagnostiikka infrastruktuurin lokitiedot. Se on ohitettuja ja muotoiltu oikein, jotta määritykset suoraan voi liittää mallin muuttujat-osassa. Katso useamman ihmisen luettavissa Esimerkki määritysten xml [Diagnostiikka määritysten rakenne](https://msdn.microsoft.com/library/azure/dn782207.aspx) .
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Yllä määrityksessä arvot definition xml-solmu on tärkeää määritysten osan se määrittää, miten aiemmin *PerformanceCounter* solmun xml-määritysten suorituskyvyn laskureita koostetaan ja tallennettu. 

> [AZURE.IMPORTANT] Nämä arvot vaikuttavat seurantaa kaavioita ja ilmoitusten Azure-portaalissa.  **Arvot** -solmu *resourceID* ja **MetricAggregation** täytyy sisältyä diagnostiikan määrityksissä, että AM, jos haluat nähdä AM seurantatiedot Azure-portaalissa. 

Seuraavassa on esimerkki xml: n arvot määritykset: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

*ResourceID* -määritettä yksilöi virtuaalikoneen-tilaukseesi. Varmista, että subscription() ja resourceGroup()-funktioiden käyttämisestä niin, että malli päivitetään automaattisesti nämä arvot tilaus ja otat käyttöön resurssiryhmän perusteella.

Jos luot useita näennäiskoneiden silmukan tarvitse täyttää copyIndex()-funktiolla voit erottaa oikein kunkin yksittäisiä AM *resourceID* -arvo. *XmlCfg* arvon päivitettäviä tukee tätä seuraavasti:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

MetricAggregation arvon *PT1H* ja *PT1M* osoittavat kooste minuutin aikana ja kooste yli tunnin.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics taulukoiden tallennustilaan

Yllä arvot määritys luo taulukoiden diagnostiikka tallennustilan-tilisi kanssa seuraavasti:

- **WADMetrics** : kaikkien WADMetrics taulukoiden etuliite
- **PT1H** tai **PT1M** : ilmaisee, että taulukossa on yli 1 tunti tai minuutin tietojen koostaminen
- **P10D** : osoittaa, että kyseessä taulukko sisältää tiedot, kun taulukon alkuun tietojen keräämisen 10 päivää
- **V2S** : merkkijonovakio
- **VVVVKKPP** : päivämäärä, jolloin taulukon aloittaa tietojen kerääminen

Esimerkki: *WADMetricsPT1HP10DV2S20151108* sisältää arvot tiedot yhdistetään yli 10 päivää 11-marraskuussa-2015 käynnistäminen tunnin    

Kukin WADMetrics taulukko sisältää seuraavat sarakkeet:

- **PartitionKey**: partitionkey muodostetaan yksilöimiseen AM resurssin *resourceID* -arvon perusteella. esimerkiksi: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : noudattaa muotoa <Descending time tick>:<Performance Counter Name>. Laskevassa jaon laskenta on enintään aika jakoviivat miinus aika, kooste kauden alussa. Esimerkiksi Jos aikana käynnistetty 10 – marraskuussa-2015 ja 00:00Hrs UTC-ajan sitten laskutoimituksen olisi: DateTime.MaxValue.Ticks - (uusi DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Jakoviivat). Varten käytettävissä tavua suorituskyvyn laskuri rivin tunnusta muistin näyttävät, kuten: 2519551871999999999__:005CMemory:005CAvailable:0020 tavua
- **CounterName** : suorituskyvyn laskuri nimi. Tämä vastaa *counterSpecifier* määritetty xml-määritys.
- **Suurin** : suorituskyvyn laskuri yhdistämisen aikana suurimman arvon.
- **Pienin** : suorituskyvyn laskuri yhdistämisen aikana pienimmän arvon.
- **Yhteensä** : suorituskyvyn laskuri kaikki arvojen summan raportoitu yhdistämisen aikana.
- **Laske** : arvojen määrä raportoidusta suorituskyvyn laskuri.
- **Keskimääräinen** : suorituskyvyn laskuri yhdistämisen aikana keskiarvo (summa ja määrä)-arvo.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Windows virtual koneen diagnostiikka tunniste valmis malli mallin [201-AM-seuranta-diagnostiikka-tunniste](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- Resurssien hallinnan malli [PowerShellin Azure](virtual-machines-windows-ps-manage.md) tai [Azure komentoriviltä](virtual-machines-linux-cli-deploy-templates.md) käyttöönotto
- Lisätietoja [authoring Azure Resurssienhallinta-mallit](../resource-group-authoring-templates.md)







