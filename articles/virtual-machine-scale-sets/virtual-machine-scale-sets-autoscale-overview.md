<properties
    pageTitle="Automaattinen skaalaus ja virtuaalikoneen skaalata joukot | Microsoft Azure"
    description="Opi käyttämään diagnostiikka- ja Automaattinen skaalaus resurssien automaattisesti skaalaamaan näennäiskoneiden asteikko joukon."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automaattinen skaalaus ja virtuaalikoneen skaalata joukot

Näennäiskoneiden asteikko joukon Automaattinen skaalaus on luominen tai poistaminen laitteiden määrittäminen vastaamaan suorituskykyä koskevat tarvittaessa. Kun työn määrä kasvaa, sovellus saattaa tarvita lisäresursseja, jotta se voi suorittaa tehtäviä tehokkaasti.

Automaattinen skaalaus on auttaa helpottamiseksi hallinta katseltavan automaattinen prosessi. Vähentämällä yleisrasite ei tarvitse jatkuvasti valvonta järjestelmän toimintaa tai päättää, miten voit hallita resursseja. Skaalaus on joustavasti prosessi. Lisää resursseja voidaan lisätä kuormituksen kasvaa, mutta kuin demand vähentää resurssien voidaan poistaa voit pienentää kustannuksia ja säilyttää suorituskykyä.

Määritä Automaattinen skaalaus järjestäminen Azure Resurssienhallinta-mallin, PowerShellin Azure, Azure CLI tai Azure portaalin asteikolla.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Määritä skaalaus Resurssienhallinta mallien avulla

Sen sijaan kaikki resurssit yhteen, koordinoidun-toiminto ottaa käyttöön sisältävän mallin käyttäminen käyttöönotto ja hallinta-sovelluksen kullekin resurssille erikseen. Valitse mallin sovelluksen resurssit on määritetty ja käyttöönoton parametrit on määritetty eri ympäristöissä. Mallin koostuu JSON ja lausekkeita, joiden avulla voit muodostaa käyttöönoton arvot. Lisätietoja on Tarkista [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md).

Malli voit määrittää kapasiteetin elementti:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Kapasiteetin tunnistaa näennäiskoneiden joukon määrän. Voit muuttaa kapasiteetti manuaalisesti ottamalla mallin odottamaasi arvoa. Jos otat mallin, jos haluat muuttaa vain kapasiteetti, voit lisätä vain SKU elementti päivitetyt kapasiteetti.

Muuttaa automaattisesti oman asteikko järjestäminen autoscaleSettings resurssi- ja diagnostiikka-tunniste yhdistelmä kapasiteetti.

### <a name="configure-the-azure-diagnostics-extension"></a>Määritä Azure diagnostiikka-tunniste

Automaattinen skaalaus voit tehdä vain jos arvot sivustokokoelman onnistuu asteikko-määrittäminen virtual jokaiseen tietokoneeseen. Azure diagnostiikka-tunniste on seuranta- ja diagnostiikka-ominaisuuksia, jotka täyttävät arvot sivustokokoelman tarpeisiin Automaattinen skaalaus-resurssin. Voit asentaa tunnisteen osana Resurssienhallinta-malli.

Tässä esimerkissä näytetään, joita käytetään muuttujien määrittäminen diagnostiikka-tunniste mallissa:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Parametrit on käytettävissä, kun malli otetaan käyttöön. Tässä esimerkissä toimitetaan tietojen tallennussijainti tallennustilan tilin nimi ja kerää tietoja, joista asteikko joukon nimi. Myös Windows Server tässä esimerkissä kerätään säikeen Laske suorituskyvyn laskuri. Kaikki käytettävissä olevat suorituskyvyn Windows-tai Linux laskureita voidaan diagnostiikka tietojen kerääminen ja tunniste-määritys voidaan lisätä.

Tässä esimerkissä näytetään tunnisteen määritelmän mallissa:

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
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

Kun diagnostiikka-tunniste suoritetaan, tiedot kerätään taulukon, joka sijaitsee tallennustilan-tili, voit määrittää. Voit etsiä kerättyjen tietojen WADPerformanceCounters-taulukossa:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>AutoScaleSettings resurssien

AutoscaleSettings resurssin käyttää diagnostiikka-tunniste tiedot päättää, suurentaa tai pienentää näennäiskoneiden asteikko-määrittäminen määrä.

Tässä esimerkissä näytetään resurssin määritysten mallissa:

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
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

Yllä olevassa esimerkissä kaksi säännöt luodaan automaattisen skaalauksen toiminnoista. Ensimmäistä sääntöä määrittää mittakaava ulos-toiminto ja toisen säännön määrittää skaalaus-toiminnon. Nämä arvot annetaan sääntöjä:

- **metricName** – tämä arvo on sama kuin suorituskyvyn laskuri, jotka olet määrittänyt wadperfcounter muuttujan diagnostiikka-tunniste. Yllä olevassa esimerkissä käytetään säikeen Laske laskuri.  
- **metricResourceUri** - arvoksi on virtuaalikoneen Skaalaa määrittäminen resurssitunnus. Tunniste on resurssiryhmän nimi, resurssin tarjoajan nimi ja määritä skaalata asteikon nimi.
- **timeGrain** – tämä arvo on tietoja, joista kerätään rakeisuuden. Edellisen esimerkin tiedot kerätään minuutin välein. Tätä arvoa käytetään timeWindow kanssa.
- **Tilasto** – tämä arvo määrittää, miten määritetty yhdistetään sopimaan automaattisen skaalauksen toiminnon. Mahdolliset arvot ovat: keskiarvo, Min, Max.
- **timeWindow** – tämä arvo on solualue, johon esiintymätiedot kerätään aika. Se on oltava 5 minuuttia-12 tuntia.
- **timeAggregation** – tämä arvo määrittää, kuinka tiedot, jotka kerätään voidaan yhdistää ajan kuluessa. Oletusarvo on keskiarvon. Mahdolliset arvot ovat: keskiarvo, vähintään, enintään, viimeksi, summa, Laske.
- **operaattori** – tämä arvo on operaattori, jota käytetään, kun haluat verrata metrisillä tiedot ja raja-arvon. Mahdolliset arvot ovat: vastaa arvoa, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
- **raja-arvon** – tämä arvo on arvo, joka käynnistää asteikko-toiminnon. Muista antaa asteikko-kohtaa toiminnon raja-arvon ja Skaalaa-toiminnon raja-arvon riitä erotuksen. Jos määrität saman arvoja, järjestelmä aikoo vakion muuttaminen, joka estää sen käyttöönoton skaalauksen toiminto. Esimerkiksi määrittäminen molemmat 600 viestiketjuissa siirtyminen edellisessä esimerkissä ei toimi.
- **suunta** – tämä arvo määrittää toiminnon, joka suoritetaan, kun raja-arvo on saavutettu. Mahdolliset arvot ovat Suurenna tai Pienennä.
- **Kirjoita** – tämä arvo on toiminto, joka suoritetaan, ja määritettävä ChangeCount tyyppi.
- **arvo** , tämä arvo on näennäiskoneiden, jotka lisätään tai poistetaan asteikko-määrittäminen määrä. Tämän arvon on oltava 1 tai suurempi.
- **cooldown** – tämä arvo on aika odottamaan skaalauksen edellisen toiminnon jälkeen ennen seuraavan toiminnon. Tämän arvon on oltava minuutin ja viikkoa.

Sen mukaan, käytössäsi on suorituskyvyn laskuri-jotkin osat mallin määrityksessä käytetään eri tavalla. Suorituskyvyn laskuri on säikeen Laske edellisessä esimerkissä, raja-arvo on 650 asteikko ulos-toiminto ja raja-arvo on 550 asteikko-toiminnon. Jos käytät laskuri, kuten prosenttia suorittimen ajasta, raja-arvo on määritetty, joka määrittää skaalauksen toiminnon suorittimen käyttö prosentteina.

Kun kuormituksen luodaan näennäiskoneiden, joka käynnistää skaalauksen toiminnon, Määritä kapasiteetin kasvaa mallin arvon perusteella. Esimerkiksi mittakaava määrittäminen, joissa kapasiteetti on määritetty 3 ja mittakaava-toiminnon arvo on määritetty 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Kun lataus on luotu, joka aiheuttaa keskimääräinen viestiketjun laskeminen Siirry 650 kynnysarvo:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

Skaalaa ulos-toiminto käynnistyy, joka aiheuttaa resursseja joukon kasvaa yhdellä:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

Ja virtual machine lisätään asteikko-määrittäminen:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Cooldown viiden minuutin ajan kuluttua Jos viestiketjuissa siirtyminen tietokoneissa määrän keskiarvo pysyy yli 600 toiseen tietokoneeseen lisätään joukkoon. Jos keskiarvo viestiketjun Laske pysyy 550 alla, asteikko-määrittäminen kapasiteettia vähennetään jokin ja koneen poistetaan joukosta.

## <a name="set-up-scaling-using-azure-powershell"></a>Määritä skaalaus Azure PowerShellin avulla

Esimerkkejä PowerShellin avulla voit määrittää automaattisen skaalauksen poistaminen, tarkista [näytön PowerShellin Azure nopeasti Käynnistä objektit](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Määritä skaalaus Azure CLI käyttäminen

Esimerkkejä määrittäminen automaattisen skaalauksen poistaminen Azure CLI avulla saat katsomalla [Azure näytön Office kaikissa ympäristöissä CLI nopeasti Käynnistä objektit](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Määritä skaalaus Azure-portaalissa

Esimerkki automaattisen skaalauksen poistaminen määrittäminen Azure portaalin avulla näkyviin Tarkista [virtuaalikoneen asteikko määrittäminen luominen käyttämällä Azure portaalin](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Tutkia skaalauksen toiminnot

- [Azure portal]() - saat tällä hetkellä rajoitetun tietojen portaalissa.
- [Azure resurssin Explorer]() - työkalun on paras tutustuminen asteikko määrittäminen nykyisen tilan. Seuraa Tämä polku ja mittakaava joukko, jonka loit esiintymän näkymän pitäisi näkyä: tilaukset > {tilauksen} > resourceGroups > {resurssiryhmän} > tarjoajat > Microsoft.Compute > virtualMachineScaleSets > {asteikko määrittäminen} > virtualMachines
- Azure PowerShell – Käytä tätä komentoa tietoja:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Yhteyden muodostaminen jumpbox virtuaalikoneen samalla tavalla kuin toiseen koneeseen ja sitten käyttöoikeus etäyhteyden näennäiskoneiden määrittäminen seurannassa yksittäisten prosessien mittakaava.

## <a name="next-steps"></a>Seuraavat vaiheet

- Tarkista, näet, miten Luo määrityksessä Automaattinen skaalaus määritetty asteikko [Skaalaa automaattisesti koneet virtuaalikoneen-asteikko-joukko](virtual-machine-scale-sets-windows-autoscale.md) .
- Etsi esimerkkejä seuranta ominaisuuksia [Azure näytön PowerShell nopeasti](../monitoring-and-diagnostics/insights-powershell-samples.md) aloitusnäytössä näytteiden Azure-näyttö
- Lue [käyttää Automaattinen skaalaus-toimintoja, sähköpostin ja webhook Azure näytön ilmoitusten ilmoitukset lähetetään](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)ilmoitus ominaisuuksista.
- Lisätietoja on artikkelissa [Lähetä sähköposti- ja webhook Azure näytön ilmoitusten ilmoituksia kirjautuu käytön valvonta](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Lisätietoja [Lisäasetukset Automaattinen skaalaus skenaarioita](./virtual-machine-scale-sets-advanced-autoscale.md).
