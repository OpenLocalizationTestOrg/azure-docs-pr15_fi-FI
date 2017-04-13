<properties
 pageTitle="Automaattinen skaalaus HPC Pack klusterisolmujen | Microsoft Azure"
 description="Automaattisesti suurennus ja pienennys Azure HPC Pack klusterin Laske solmujen määrän"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automaattisesti suurennus ja pienennys Azure HPC Pack klusteriresurssien mukaan klusterin työmäärää




Jos otat Azure "purskeen" solmujen HPC Pack-klusterin tai HPC Pack-klusterin luominen Azure VMs, haluat ehkä vielä automaattisesti Suurenna tai Pienennä Azure Laske resurssit, kuten solmujen tai sydämiä mukaan nykyisen työmäärää klusterin määrä. Näin voit käyttää Azure resurssien tehokkaammin sekä hallita niiden kustannukset.
Määrittäminen HPC Pack-klusterin ominaisuuden **AutoGrowShrink**. Vaihtoehtoisesti voit suorittaa **AzureAutoGrowShrink.ps1** HPC PowerShell-komentosarja, joka on asennettu HPC Pack.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lisäksi tällä hetkellä voit voit automaattisesti vain suurennus ja pienennys HPC Pack Laske solmujen, joissa on käytössä Windows Server-käyttöjärjestelmä.

## <a name="set-the-autogrowshrink-cluster-property"></a>Ominaisuuden AutoGrowShrink klusteri

### <a name="prerequisites"></a>Edellytykset

* **HPC Pack 2012 R2: n päivitys 2 tai uudempi klusterin** - klusterin pään solmu voidaan ottaa käyttöön joko paikallisessa tai Azure-AM. Katso pään paikallisen-solmu ja Azure "purskeen" solmujen aloittaminen [hybrid klusterin HPC Pack määrittäminen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) . Katso [HPC Pack IaaS käyttöönoton komentosarja](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) , joka ottaa nopeasti HPC Pack-klusterin Azure VMs.


* **Azure pään solmun varustetun klusterin** – Jos HPC Pack IaaS käyttöönoton komentosarja klusterin luominen, klusterin määritystiedoston AutoGrowShrink-asetus käyttöön **AutoGrowShrink** klusterin-ominaisuuden avulla. Lisätietoja on artikkelissa mukana [latauksen](https://www.microsoft.com/download/details.aspx?id=44949)ohjeissa. 

    Voit myös käyttöön **AutoGrowShrink** klusterin ominaisuuden, kun otat klusterin käyttämällä HPC PowerShell-komennot, joka on kuvattu seuraavassa osassa. Valmistelemisesta tämä on tehtävä seuraavat toimet:
    1. Azure hallinta-varmenteen määrittäminen, pää solmu ja Azure tilaus. Testaa käyttöönottoa käyttää oletus Microsoft HPC Azure itse allekirjoitettua varmennetta, HPC Pack asentaa pään solmu ja sertifikaatin lataaminen ainoastaan Azure tilauksen. Asetukset ja ohjeita on artikkelissa [TechNet-kirjaston ohjeita](https://technet.microsoft.com/library/gg481759.aspx).
    2. Suorita **regedit** pään solmun, siirry HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo ja lisätä uuden merkkijonoarvon. Määritä "Allekirjoitus" arvon nimen ja arvon data vaiheessa 1 varmenteen allekirjoitus.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>HPC PowerShell-komennoilla AutoGrowShrink-ominaisuuden määrittäminen

Seuraavat ovat esimerkki HPC PowerShell-komennoilla voit määrittää **AutoGrowShrink** ja säätämällä käyttäytyminen muita parametreilla. Katso [AutoGrowShrink parametrien](#AutoGrowShrink-parameters) täydellinen luettelo tämän artikkelin asetuksia. 

Nämä komennot Aloita HPC PowerShell pään solmun klusterin järjestelmänvalvojana.

**Jos haluat ottaa käyttöön AutoGrowShrink-ominaisuus**

    Set-HpcClusterProperty –EnableGrowShrink 1

**Jos haluat poistaa käytöstä AutoGrowShrink-ominaisuus**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Voit muuttaa Suurenna tunnissa**

    Set-HpcClusterProperty –GrowInterval <interval>

**Voit muuttaa Pienennä tunnissa**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**Jos haluat tarkastella AutoGrowShrink nykyiset määritykset**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink parametrit

Seuraavassa on AutoGrowShrink parametrit, joita voit muokata **Määrittäminen HpcClusterProperty** -komennon avulla.

* **EnableGrowShrink** - valitsimen käyttöön ottaminen tai käytöstä **AutoGrowShrink** -ominaisuus.
* **ParamSweepTasksPerCore** - määrä kasvaa yksi core parametrien tutkimisen tehtäviä. Oletusarvo on kasvattamista yhden core tehtävää kohden. 
 
    >[AZURE.NOTE] HPC Pack QFE KB3134307 muuttuu **ParamSweepTasksPerCore** **TasksPerResourceUnit**. Se perustuu resurssin työlaji ja voivat olla solmu, socket tai core.
    
* **GrowThreshold** - jonossa olevien tehtävien raja käynnistettävän automaattinen kasvu. Oletusarvo on 1, mikä tarkoittaa sitä, jos jonossa tila on vähintään 1 tehtävät automaattisesti kasvaa solmujen.
* **GrowInterval** - minuutteina käynnistettävän automaattinen kasvu. Oletusarvo on 5 minuuttia.
* **ShrinkInterval** - minuutteina käynnistettävän automaattinen pienentäminen. Oletusarvo on 5 minuuttia. |
* **ShrinkIdleTimes** - jatkuva tarkistukset pienentämiseksi solmut kuuluvien käyttämättömänä määrä. Oletusarvo on 3 kertaa. Esimerkiksi jos **ShrinkInterval** on 5 minuuttia, HPC Pack tarkistaa, onko solmu käyttämättömänä 5 minuutin välein. Jos solmut ovat idle-tilassa, kun 3 jatkuva tarkistaa (15 minuuttia), HPC Pack pienenee solmun.
* **ExtraNodesGrowRatio** - solmujen kasvattamista viestin kulkeva Interface (MPI) töiden uusia prosentteina. Oletusarvo on 1, mikä tarkoittaa, että HPC Pack kasvaa solmujen MPI töiden 1 %. 
* **GrowByMin** - valitsin, joka ilmaisee, onko kasvamaan automaattisesti käytäntö perustuu pienin työn tarvittavat resurssit. Oletusarvo on EPÄTOSI, mikä tarkoittaa, että HPC Pack kasvaa solmujen töiden tarvittavat työt suurin resurssien mukaan.
* **SoaJobGrowThreshold** - SOA pyynnöt käynnistettävän automaattisen raja-arvo kasvaa prosessi. Oletusarvo on 50000.  
    
    >[AZURE.NOTE] Tämä parametri on tuettu HPC Pack 2012 R2: n päivitys 3 alkaen.
    
* **SoaRequestsPerCore** -pyyntöjen saapuvien SOA määrä kasvaa yksi core. Oletusarvo on 20000.  

    >[AZURE.NOTE] Tämä parametri on tuettu HPC Pack 2012 R2: n päivitys 3 alkaen.

### <a name="mpi-example"></a>MPI-Esimerkki

Oletusarvoisesti HPC Pack kasvaa 1 % ylimääräisiä solmujen MPI töiden (**ExtraNodesGrowRatio** on määritetty 1). Syy on, että MPI saattaa edellyttää useiden solmujen ja työ voidaan suorittaa vain, kun kaikki solmut ovat valmiit. Azure käynnistyessä solmujen toisinaan yksi solmu on ehkä kauemmin kuin muiden ilmenneet solmujen on ollut käyttämättömänä odotettaessa kyseisen solmu voit aloittaa Aloita. Kasvava ylimääräisiä solmujen HPC Pack vähentää tämän resurssin Odotusaika ja tallentaa mahdollisesti kustannukset. Kun haluat lisätä ylimääräisen solmujen MPI töitä (esimerkiksi 10 %) prosentteina, samalla komennon suorittaminen

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA-Esimerkki

Oletusarvon mukaan **SoaJobGrowThreshold** on määritetty 50000 ja **SoaRequestsPerCore** on määritetty 200000. Jos lähetät yksi SOA työ on 70000 pyyntöjä, ole yhtä jonossa tehtävää ja pyynnöt ovat 70000. Tässä tapauksessa HPC Pack kasvaa 1 core jonossa tehtävän ja pyynnöt, kasvaa (70000 50000) / core 20000 = 1, jotta yhteensä kasvaa 2 sydämiä SOA työlle.

## <a name="run-the-azureautogrowshrinkps1-script"></a>AzureAutoGrowShrink.ps1-komentosarjan suorittaminen

### <a name="prerequisites"></a>Edellytykset

* **HPC Pack 2012 R2: n päivitys 1 tai uudempi klusterin** - **AzureAutoGrowShrink.ps1** komentosarja on asennettu % CCP_HOME % bin-kansio. Klusterin pään solmu voidaan ottaa käyttöön joko paikallisessa tai Azure-AM. Katso pään paikallisen-solmu ja Azure "purskeen" solmujen aloittaminen [hybrid klusterin HPC Pack määrittäminen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) . Katso [HPC Pack IaaS käyttöönoton komentosarja](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) ottamaan nopeasti HPC Pack-klusterin Azure VMs- tai käyttää [Azure pikaopas malli](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **PowerShellin azure 0.8.12** - komentosarja tällä hetkellä määräytyy PowerShellin Azure tietyn versiossa. Jos käytät pään solmun uudempi versio, voit joutua PowerShellin Azure muuntaminen [versio 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) komentosarjan suorittamista. 

* **Klusterin Azure purskeen solmujen kanssa** – Suorita komentosarja, johon HPC kielipaketti on asennettu asiakastietokoneeseen tai pään solmun. Jos asiakkaan tietokoneessa, varmista, että määrität muuttujan $env: CCP_SCHEDULER oikein osoittamaan pään solmu. Azure-purskeen"solmut on jo lisättävä klusterin, mutta ne voivat olla tiedot ei käytössä-tilassa.


* **Klusterin käyttöön Azure VMs** - suorittamalla komentosarja pään solmun AM, koska se on riippuvainen **Käynnistä HpcIaaSNode.ps1** ja **Lopeta HpcIaaSNode.ps1** komentosarjojen, joka on asennettu siellä. Komentosarjoja lisäksi edellyttävät Azure hallinta-varmenteen tai julkaista asetustiedosto (katso [Manage Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Varmista, että kaikki Laske-solmu VMs tarvitset on jo lisätty klusteriin. Ne voivat olla pysäytetty-tilassa.

### <a name="syntax"></a>Syntaksi

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parametrit

 * **NodeTemplates** - solmu-mallien nimet solmujen suurennus ja pienennys, Määritä. Jos ei määritetä (oletusarvo on @()), **AzureNodes** solmu-ryhmässä kaikki solmut ovat laajuus, kun **NodeType** on AzureNodes arvo ja kaikki solmut **ComputeNodes** solmu-ryhmässä käyttämässä laajuus **NodeType** on ComputeNodes arvo.

 * **JobTemplates** - mallit nimet Määritä solmujen kasvaa.

 * **NodeType** - solmu suurennus ja pienennys tyyppi. Tuetut arvot ovat:

     * **AzureNodes** – paikallisen solmujen Azure PaaS (purske) tai Azure IaaS klusterin.

     * **ComputeNodes** - vain Azure IaaS-klusterin-solmun Laske VMs.

* **NumOfQueuedJobsPerNodeToGrow** - määrä kasvaa yksi solmu pakollinen jonossa olevat työt.

* **NumOfQueuedJobsToGrowThreshold** - jonossa olevat työt Suurenna Aloita kynnysarvo määrän.

* **NumOfActiveQueuedTasksPerNodeToGrow** - aktiivinen pakollinen kasvattamista yhden solmun jonossa olevien tehtävien määrä. Jos **NumOfQueuedJobsPerNodeToGrow** on määritetty, joiden arvo on suurempi kuin 0, tämä parametri jätetään huomiotta.

* **NumOfActiveQueuedTasksToGrowThreshold** - onnistuneiden yritysten Suurenna Aloita aktiiviset jonossa olevat tehtävät.

* **NumOfInitialNodesToGrow** - alkuperäinen vähimmäismäärä solmujen kasvaa, jos kaikki alueen solmut **Ei otettu käyttöön** tai **pysäytetty (Deallocated)**.

* **GrowCheckIntervalMins** - minuutteina tarkistukset kasvattamista välillä.

* **ShrinkCheckIntervalMins** - minuutteina tarkistaa Pienennä välillä.

* **ShrinkCheckIdleTimes** - jatkuva määrän Pienennä tarkistukset (erotettuina **ShrinkCheckIntervalMins**) solmut kuuluvien käyttämättömänä.

* **UseLastConfigurations** - argumentin tiedosto tallennetaan aiemman määrityksiä.

* **ArgFile**- käyttää Tallenna ja Päivitä määrityksiä komentosarjan suorittaminen argumentin tiedoston nimi.

* **LogFilePrefix** - lokitiedosto etuliite. Voit määrittää polun. Oletusarvoisesti loki kirjoitetaan nykyiseen toimimasta hakemistoon.

### <a name="example-1"></a>Esimerkki 1

Seuraava esimerkki määrittää Azure purskeen solmut mallilla oletus AzureNode suurennus ja pienennys automaattisesti käyttöön. Jos kaikki solmut on alun perin tila **Ei ole otettu käyttöön** , ainakin 3 solmujen käynnistetään. Jos jonossa olevat työt on suurempi kuin 8, komentosarja käynnistyy solmujen, kunnes niiden ylittää jonossa olevat työt **NumOfQueuedJobsPerNodeToGrow**suhde. Jos solmu löydy käyttämättömänä peräkkäiset käyttämättömänä 3 kertaa, se pysäytetään.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Esimerkki 2

Seuraava esimerkki määrittää Azure Laske-solmu VMs mallilla oletus ComputeNode suurennus ja pienennys automaattisesti käyttöön.
Työn oletusmallin määrittämän työt Määritä klusterin työmäärää. Jos kaikki solmut alun perin pysäytetty, vähintään 5 solmujen käynnistetään. Jos aktiivinen jonossa olevien tehtävien määrä ylittää 15, komentosarja käynnistyy solmujen ennen niiden ylittää aktiivisen jonossa olevien tehtävien **NumOfActiveQueuedTasksPerNodeToGrow**suhteen. Jos solmu löydy käyttämättömänä peräkkäiset käyttämättömänä 10 kertaa, se pysäytetään.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
