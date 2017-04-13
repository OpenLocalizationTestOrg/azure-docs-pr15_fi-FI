<properties
   pageTitle="Lokien kerääminen Azure Diagnostiikan avulla | Microsoft Azure"
   description="Tässä artikkelissa käsitellään määrittäminen Azure diagnostiikka kerää lokit käynnissä Azure-palvelu kangasta klusterista."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Lokien kerääminen Azure Diagnostiikan avulla

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Kun käytät Azure palvelun kangasta-klusterin-on hyvä lokit kerätään kaikki solmut keskitetyssä paikassa. Ottaa lokit keskitetysti avulla voit analysoida ja yhteyttä klusterin ongelmat ja sovelluksia ja palveluita, klusterin ongelmat vianmääritys.

Lataa ja lokien kerääminen yksi tapa on Azure diagnostiikka-tunniste, jossa Lataa lokit Azure-tallennustilan. Lokit eivät ole että hyötyä suoraan tallennustilan. Mutta ulkoinen prosessin avulla voit lukea tapahtumat tallennustilan ja sijoittaa ne tuotteissa, kuten [Log Analytics](../log-analytics/log-analytics-service-fabric.md), [Joustavasti haun](service-fabric-diagnostic-how-to-use-elasticsearch.md)tai lokin jäsennyksen ratkaisu.

## <a name="prerequisites"></a>Edellytykset
Jotkin toiminnot suorittavat tässä asiakirjassa työkaluja käytetään:

* [Azure diagnostiikka](../cloud-services/cloud-services-dotnet-diagnostics.md) (Azure pilvipalveluihin liittyvät mutta on hyvä tiedot ja esimerkit)
* [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](../powershell-install-configure.md)
* [Azure Resurssienhallinta-asiakas](https://github.com/projectkudu/ARMClient)
* [Azure Resurssienhallinta-malli](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Log-lähteistä, jonka haluat kerätä
- **Palvelun kangasta lokit**: lähettämän-ympäristön vakio tapahtuman jäljitys for Windows (tapahtumien seuranta) ja EventSource kanavat. Lokeja voi olla useita lajia:
  - Toiminnallisia tapahtumien: toiminnoista, joita palvelun kangasta platform suorittaa lokitiedot. Esimerkkejä tällaisista tiedostoista ovat luominen sovellusten ja palveluiden, solmu tila muuttuu ja Päivitä tiedot.
  - [Luotettavan toimijoiden ohjelmointi mallin tapahtumat](service-fabric-reliable-actors-diagnostics.md)
  - [Luotettavan Services programming mallin tapahtumat](service-fabric-reliable-services-diagnostics.md)
- **Sovelluksen tapahtumien**: tapahtumien lisääminen palvelukoodi peräisin ja kirjoitetut EventSource helper luokan annettu Visual Studio-mallien avulla. Katso lisätietoja siitä, miten voit kirjoittaa lokit sovelluksestasi [näyttö ja vianmäärityksen services paikallisesta kehittäminen asetuksissa](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>Ottaa käyttöön diagnostiikka-tunniste
Ensimmäinen vaihe lokien kerääminen on ottaa käyttöön jokaisen palvelun kangasta klusterin VMs diagnostiikka-tunniste. Diagnostiikka-tunniste kerää kunkin AM-lokit ja lataa ne tallennustilan-tili, voit määrittää. Etenee hieman mukaan, käytätkö Azure portal tai Azure Resurssienhallinta. Myös etenee käyttöönottoon kuuluu klusterin luominen vai on aiemmin luodun klusterin mukaan. Katsotaan kunkin skenaarion vaiheet.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Ottaa käyttöön portaalin klusterin luonnista osana diagnostiikka-tunniste
Käyttöön diagnostiikka-tunniste klusterin VMs osana klusterin luominen, voit käyttää diagnostiikan asetukset-paneeli seuraavan kuvan mukaisesti. Luotettava toimijat tai luotettavia palveluja tapahtuman sivustokokoelman käyttöön Varmista, että Diagnostiikka on määritetty **käyttöön** (oletusasetus). Kun olet luonut klusterin, et voi muuttaa näitä asetuksia portaalissa.

![Azure diagnostiikan asetusten portaalissa klusterin luontia varten](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Azure tuki ryhmän *edellyttää* tuki kirjaa minkä tahansa tukipyyntöjä, jonka luot ratkaisemiseksi. Lokitiedostot kerätään reaaliajassa, ja se tallennetaan luotu resurssiryhmän tallennustilan tunnuksilla. Diagnostiikan asetusten määrittäminen sovellustason tapahtumat. Näitä tapahtumia ovat [Luotettavia toimijoiden](service-fabric-reliable-actors-diagnostics.md) tapahtumat, [Luotettava palvelujen](service-fabric-reliable-services-diagnostics.md) tapahtumat ja jotkin järjestelmän tason palvelun kangasta tapahtumat tallennetaan Azure-tallennustilan.

Tuotteissa, kuten [Joustavasti haun](service-fabric-diagnostic-how-to-use-elasticsearch.md) tai oman prosessin saa tapahtumat tallennustilan tilin. Tällä hetkellä ei voi suodattaa tai ruokota tapahtumat, jotka on tullut taulukko. Jos et toteuta prosessi, jos haluat poistaa tapahtumat-taulukosta, taulukon jatkaa kasvaa.

Klusterin luomisen avulla portaalin, on erittäin suositeltavaa, että lataat mallin *ennen kuin valitset * *OK*** klusterin luomiseen. Lisätietoja on viitata [määrittäminen palvelun kangasta klusterin Azure Resurssienhallinta-mallin avulla](service-fabric-cluster-creation-via-arm.md). Tarvitset mallia, voit tehdä muutoksia myöhemmin, koska et voi tehdä muutoksia käyttämällä portaalin.

Voit viedä mallit-portaalista seuraavien ohjeiden mukaisesti. Malleja voi olla vaikeaa, koska ne on ehkä null-arvoja, jotka puuttuu tarvittavat tiedot.

1. Avaa resurssiryhmä.
2. Valitse Näytä asetukset-paneeli **asetukset** .
3. Valitse **ominaisuuksissa** näyttämään käyttöönoton historia-paneeli.
4. Valitse käyttöönoton käyttöönoton tietojen esittämiseen.
5. Valitse **Vietävä malli** näyttämään malli-ruudussa.
6. Valitse Vie .zip-tiedosto, joka sisältää mallin, parametrin ja PowerShell-tiedostojen **tallentaminen tiedostoon** .

Kun olet vienyt tiedostot, sinun täytyy tehdä muutoksen. Muokkaa parameters.json-tiedostoa ja poista **adminPassword** elementti. Tämä aiheuttaa Kysy salasana käyttöönoton komentosarja suoritetaan. Kun käytössäsi on käyttöönoton komentosarja, voit joutua korjaamaan null-parametriarvot.

Voit päivittää kokoonpano lataamasi malli toimimalla seuraavasti:

1. Pura sisältö paikallisessa tietokoneessa olevaan kansioon.
2. Muokkaa sisältöä vastaamaan uudet määritykset.
3. PowerShellin aloitus- ja siirry kansioon, johon purit sisältöä.
4. Suorita **deploy.ps1** ja täytä tilauksen tunnus, resurssi-ryhmänimi (Käytä samalla nimellä Päivitä konfigurointi) ja käyttöönoton yksilöivä nimi.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Klusterin luominen osana diagnostiikka-tunniste asentavat Azure resurssien hallinta
Voit luoda klusterin Resurssienhallinta, tarvitset lisää diagnostiikka-määrityksen JSON koko klusterin Resurssienhallinta-malliin, ennen kuin luot klusterin. Annamme lisätty Microsoftin Resurssienhallinta mallia näytteiden osana diagnostiikan määrityksissä otoksen viisi AM klusterin Resurssienhallinta mallia. Voit katsoa Azure näytteiden valikoiman tähän sijaintiin: [viisi solmun klusterin diagnostiikan Resurssienhallinta mallin otoksen kanssa](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Saat näkyviin Resurssienhallinta-mallin diagnostiikka-asetuksen, Avaa azuredeploy.json tiedosto ja Etsi **IaaSDiagnostics**. Voit luoda tämän mallin avulla klusterin, valitse osoitteessa edellisen linkin **käyttöönotto Azure** -painike.

Vaihtoehtoisesti voit ladata Resurssienhallinta-malli, tehdä siihen muutoksia ja luoda klusterin muokatun mallin avulla `New-AzureRmResourceGroupDeployment` komennon PowerShellin Azure-ikkunassa. Katso seuraava koodi parametrien, jotka siirtävät komennon avulla. Lisätietoja siitä, miten voit ottaa resurssiryhmä PowerShellin avulla on artikkelissa [käyttöönotto resurssiryhmä Azure Resurssienhallinta-malli](../resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Ottaa käyttöön aiemmin luodun klusterin diagnostiikka-tunniste
Jos sinulla on aiemmin luodun klusterin, joka ei ole otettu käyttöön diagnostiikan tai jos haluat muokata olemassa olevaa määritystä, voit lisätä tai päivittää sen. Muokkaa Resurssienhallinta-malli, jota käytetään aikaisemmin luodun klusterin luomiseen ja Lataa malli-portaalista kuvatulla. Muokkaa template.json tiedoston suorittamalla seuraavat toimet.

Lisää uusi tallennustilan resurssi mallia lisäämällä resurssit-osa.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Lisää seuraavaksi parametrit-osaan vain tallennustilan tilin-määritysten jälkeen välillä `supportLogStorageAccountName` ja `vmNodeType0Name`. Korvaa *tallennustilan tilin nimi tähän* paikkamerkki-teksti-tallennustilan tilin nimi.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Päivitä sitten `VirtualMachineProfile` template.json tiedoston lisäämällä tunnisteet matriisista seuraava koodi-kohtaan. Muista lisätä pilkun alkuun tai loppuun, sen mukaan, missä se on lisätty.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Kun olet muokannut tiedostoa template.json kuvatulla tavalla, Julkaise Resurssienhallinta-malli. Jos malli on viety, deploy.ps1 tiedoston suorittaminen julkaisee kirjan uudelleen mallia. Kun otat käyttöön, varmista, että **ProvisioningState** on **onnistui**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Voit kerätä ja lähettämään lokit myös uusi EventSource lähteiden diagnostiikka päivittäminen
Päivitä diagnostiikka lokit kerätään uusi EventSource kanavat, jotka edustavat uusi sovellus, jossa olet avaamassa käyttöönotto, suorittaa samoja ohjeita kuin [edellisessä osassa](#deploywadarm) aiemmin luodun klusterin asetukset-kansio.

Päivitä `EtwEventSourceProviderConfiguration` template.json tiedostoa, johon haluat lisätä merkintöjä, uusi EventSource kanavien ennen kuin voit käyttää asetuksia päivittää käyttämällä-osassa `New-AzureRmResourceGroupDeployment` PowerShell-komentoa. Lähteen nimi on määritetty osana koodisi Visual Studiossa luodut ServiceEventSource.cs-tiedostossa.

Jos tapahtuma tietolähteen nimi on oma Eventsource, Lisää seuraava koodi voit sijoittaa omat Eventsource tapahtumia MyDestinationTableName-taulukon.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Kerääminen suorituskyvyn laskureita tai tapahtumalokien, Muokkaa Resurssienhallinta-mallia käyttämällä esimerkkejä annettu luominen [Windowsin virtual tietokoneessa, jossa seuranta- ja diagnostiikka Azure Resurssienhallinta-mallin avulla](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md). Valitse Julkaise Resurssienhallinta-malli.

## <a name="next-steps"></a>Seuraavat vaiheet
Tarkemmin mitä tapahtumien pitäisi näyttää kun vianmäärityksestä on artikkelissa lähettämän [Luotettava toimijoiden](service-fabric-reliable-actors-diagnostics.md) ja [Luotettavia palveluja](service-fabric-reliable-services-diagnostics.md)diagnostiikan tapahtumat.


## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
* [Opettele suorituskyvyn laskureita tai lokien kerääminen käyttämällä diagnostiikka-tunniste](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Lokitiedoston Analytics palvelun kangasta ratkaisu](../log-analytics/log-analytics-service-fabric.md)
