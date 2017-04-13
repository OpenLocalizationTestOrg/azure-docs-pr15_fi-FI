<properties
   pageTitle="Testattavuus toiminnon | Microsoft Azure"
   description="Tässä artikkelissa on lyhyt löytyvät Microsoft Azure palvelun kangasta testattavuus toiminnot."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Testattavuus toiminnot
Jotta voit simuloida epäluotettavista infrastruktuuri, Azure palvelun kangasta avulla voit developer eri simuloida eri tosielämän virheet ja tilan siirrot. Nämä ovat näkyvät testattavuus toimenpiteitä. Toiminnot ovat alatason API, joka aiheuttaa tietyn vika lisäämisen, tilan siirtymä tai vahvistus. Yhdistämällä nämä toiminnot, voit kirjoittaa täydellinen testi skenaariot palvelujen.

Palvelun kangasta on joitakin yleisiä testi tilanteita, joissa koostuu näistä toiminnoista. On erittäin suositeltavaa käyttää valmiita tilanteista, joka valitaan huolellisesti Testaa yleisiä tilan siirtymien ja tapauksissa epäonnistumisen. Kuitenkin toimintoja voidaan mukautetun testiskenaarioiden luominen, kun haluat lisätä skenaarioita, joka ei koske valmiin käyttötavoista vielä tai, jotka ovat mukautetun sovelluksen räätälöity kattavuus.

C# käyttöotot toiminnot löytyvät System.Fabric.dll kokoonpanon. Järjestelmän kangasta PowerShell-moduulin löytyy Microsoft.ServiceFabric.Powershell.dll kokoonpanon. ServiceFabric PowerShell-moduulin asennetaan osana runtime-ohjelmiston asennus helppous varten.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Kaksivaiheista ja äkillisen vika toiminnot
Kaksi tärkeintä uudelleenkäytettävät luokitellaan testattavuus toiminnot:

* Äkillisen virheitä: nämä virheet simuloida virheet, kuten tietokoneen käynnistyy ja käsitellä kaatuu. Tässä tapauksessa on toistunut prosessin suorittamisen yhteydessä sulkeutuu odottamatta. Tämä tarkoittaa ei uudelleenjärjestäminen valtion suorittamisen, ennen kuin sovellus käynnistyy uudelleen.

* Kaksivaiheista virheitä: nämä virheet simuloida kaksivaiheista toiminnot, kuten replikan siirtää ja tippaa käynnistämä kuormituksen tasaamisen. Tässä tapauksessa palvelu saa ilmoituksen Sulje ja voit tyhjentää tilan ennen kuin suljet.

Parempi laatu kelpoisuuden pitää palvelu- ja työmäärää samalla aiheuttaa eri kaksivaiheista ja äkillisen virheitä. Äkillisen virheitä on syytä olla missä service-prosessi odottamatta loppuu keskellä jotkin työnkulun skenaarioita. Tämä testaa palautus-polku, kun se palvelu on palautettu palvelun kangasta mukaan. Tämä auttaa testata tietojen yhdenmukaisuuden ja onko palvelun tilan oikeanlaista virheen jälkeen. Muut joukko virheet (kaksivaiheista virhettä) Testaa, että palvelun reagoi oikein siirretään palvelun kangasta replikoita. Tämä testaa käsittely peruutuksesta RunAsync-menetelmää. Palvelu on tarkistettava on määritetty, Tallenna oikein palvelintilaan peruutus-tunnuksen ja Lopeta RunAsync-menetelmää.

## <a name="testability-actions-list"></a>Testattavuus toimintoluettelo

| Toiminto | Kuvaus | Hallitun Ohjelmointirajapinnan | PowerShell cmdlet-komento | Kaksivaiheista/äkillisen virheitä |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Poistaa virheelliset Sammuta testi ohjaimen Jos klusterin testin tila. | CleanTestStateAsync | Poista ServiceFabricTestState | Ei käytettävissä |
| InvokeDataLoss | Aiheuttaa tietojen menettämisen service-osioon. | InvokeDataLossAsync | Käynnistää ServiceFabricPartitionDataLoss | Kaksivaiheista |
| InvokeQuorumLoss | Siirtää quorum tappio annetun tilallisten service-osio. | InvokeQuorumLossAsync | Käynnistää ServiceFabricQuorumLoss | Kaksivaiheista |
| Siirrä ensisijainen | Siirtyy määritetyn klusterisolmu tilallisten palvelun se määritetyn ensisijainen. | MovePrimaryAsync | Siirrä ServiceFabricPrimaryReplica | Kaksivaiheista |
| Siirrä toissijainen | Siirtyy eri klusterisolmu tilallisten palvelun se nykyisen toissijaisen. | MoveSecondaryAsync | Siirrä ServiceFabricSecondaryReplica | Kaksivaiheista |
| RemoveReplica | Varmistaminen poistamalla replikan klusterista replikan epäonnistui. Näin se suljetaan ja siirtymä se rooli "Ei mitään", poistetaan kaikki sen tilat klusterista. | RemoveReplicaAsync | Poista ServiceFabricReplica | Kaksivaiheista |
| RestartDeployedCodePackage | Varmistaminen koodin paketin klusterin solmu käyttöön käynnistämällä koodin paketin prosessin epäonnistui. Tämä keskeyttää koodin paketin prosessi, joka käynnistyy kaikki käyttäjän palvelun replikat isännöidään vaiheisiin. | RestartDeployedCodePackageAsync | Uudelleenkäynnistys ServiceFabricDeployedCodePackage | Äkillisen |
| RestartNode | Varmistaminen käynnistämällä solmu palvelun kangasta klusterin solmu epäonnistui. | RestartNodeAsync | Uudelleenkäynnistys ServiceFabricNode | Äkillisen |
| RestartPartition | Varmistaminen palvelinkeskuksen pimennysaika tai klusterin pimennysaika tilanne käynnistämällä jotkin tai kaikki replikoida sisältävän osion. | RestartPartitionAsync | Uudelleenkäynnistys ServiceFabricPartition | Kaksivaiheista |
| RestartReplica | Varmistaminen replikan epäonnistui uudelleenkäynnistyksen pysyviä replikan klusterin, se sulkemalla ja avaamalla sen sitten uudelleen. | RestartReplicaAsync | Uudelleenkäynnistys ServiceFabricReplica | Kaksivaiheista |
| StartNode | Aloittaa solmu klusteriin, joka on jo pysäytetty. | StartNodeAsync | Käynnistä ServiceFabricNode | Ei käytettävissä |
| StopNode | Varmistaminen mukaan pysäyttäminen klusterin solmu solmu epäonnistui. Solmun säilyvät alaspäin, kunnes StartNode kutsutaan. | StopNodeAsync | Lopeta ServiceFabricNode | Äkillisen |
| ValidateApplication | Tarkistaa käytettävyyttä ja kuntoa palvelun kangasta palvelun sovelluksesta yleensä jälkeen aiheuttaa joidenkin vika järjestelmään. | ValidateApplicationAsync | Testaa ServiceFabricApplication | Ei käytettävissä |
| ValidateService | Voit tarkistaa käytettävyyttä ja kuntoa palvelun kangasta-palvelun yleensä jälkeen aiheuttaa joidenkin vika järjestelmään. | ValidateServiceAsync | Testaa ServiceFabricService | Ei käytettävissä |

## <a name="running-a-testability-action-using-powershell"></a>Testattavuus-toiminnon PowerShellin avulla

Tässä opetusohjelmassa näytetään, miten voit suorittaa testattavuus-toiminnon PowerShell-toiminnolla. Opit testattavuus-toiminnon suorittaminen paikallisen (yksi-valinta)-klusterin tai Azure klusterin vastaan. Microsoft.Fabric.Powershell.dll--palvelun kangasta PowerShell-moduulin--asennetaan automaattisesti, kun asennat Microsoft Service kangasta MSI. Moduuli ladataan automaattisesti, kun avaat PowerShell-kehotteessa.

Opetusohjelman osat:

- [Suorittaa toiminnon yhden klusterin](#run-an-action-against-a-one-box-cluster)
- [Suorittaa toiminnon Azure klusterin](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Suorittaa toiminnon yhden klusterin

Voit suorittaa testattavuus-toiminnon paikallisen klusterin vastaan, ensin muodostaa yhteyttä klusterin ja avaa PowerShell-kehotteessa järjestelmänvalvojan oikeuksia. Tarkista uutiskirjeistä **Uudelleenkäynnistyksen ServiceFabricNode** -toiminto.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Tätä toimintoa **Uudelleen ServiceFabricNode** suoritetaan solmu nimeltä "Node1". Valmistumisen tila määrittää, että se ei tarkista onko uudelleenkäynnistyksen solmu toiminnon todella onnistui. Kun "Tarkista" aiheuttaa se tarkistaa, onko uudelleen-toiminto onnistui todella, joka määrittää valmistuminen-tilassa. Sen sijaan, että määrität suoraan solmun sen nimen mukaan, voit määrittää sen kautta osio-näppäintä ja replikan tyyppi seuraavasti:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Uudelleenkäynnistys ServiceFabricNode** käytetään käynnistämään palvelun kangasta solmu klusterin. Tämä estää Fabric.exe-prosessi, jossa uudelleen kaikki järjestelmän palvelu ja käyttäjän palvelun replikat isännöimät solmun. Tämä Ohjelmointirajapinnan käyttäminen palvelun testaaminen auttaa paljastaa virheet automaattisesti palautus polut pitkin. Sen avulla voit simuloida klusterin solmu virheet.

Seuraavassa näyttökuvassa näkyy **Uudelleenkäynnistyksen ServiceFabricNode** testattavuus-komento-toiminto.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Ensimmäinen **Get-ServiceFabricNode** (cmdlet palvelun kangasta PowerShell-moduulin) tulos näyttää paikallisen klusterin on viisi solmujen: Node.1 Node.5. Kun (cmdlet-komento) **Uudelleenkäynnistyksen ServiceFabricNode** testattavuus-toiminto on suoritettu solmu nimeltä Node.4, näemme solmun käytettävyyttä on vaihdettu.

### <a name="run-an-action-against-an-azure-cluster"></a>Suorittaa toiminnon Azure klusterin

Testattavuus-toiminnon suorittaminen (käyttämällä PowerShell) vasten Azure klusterin eroaa toiminnon suorittaminen paikallisen klusterin vasten. Ainoa ero on, ennen kuin voit suorittaa toiminnon sijaan paikallisen klusterin muodostamisesta sinun on muodostettava Azure klusterin ensin.

## <a name="running-a-testability-action-using-c35"></a>Testattavuus-toiminnon käyttäminen C & #35;

Voit suorittaa testattavuus-toiminnon avulla C#, sinun on ensin yhteyden klusterin FabricClient avulla. Valitse Hanki toiminnon suorittamiseksi parametrit. Parametrien avulla voidaan suorittaa saman toiminnon.
Katsoo RestartServiceFabricNode-toiminto toimii puhelimessasi yksi tapa on käyttämällä klusterin solmutietoja (Solmunimi ja solmun Esiintymätunnus).

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Parametrin kuvaus:

- **CompleteMode** määrittää, että tila ei tarkista onko uudelleen-toiminto todella onnistui. Kun "Tarkista" aiheuttaa se tarkistaa, onko uudelleen-toiminto onnistui todella, joka määrittää valmistuminen-tilassa.  
- **OperationTimeout** asettaa toiminto on valmis ennen TimeoutException poikkeuksen ajan.
- **CancellationToken** mahdollistaa odottavan kutsun voi peruuttaa.

Sen sijaan, että määrität suoraan solmun sen nimen mukaan, voit määrittää sen kautta osio-näppäintä ja replikan tyyppi.

Lisätietoja on artikkelissa [PartitionSelector ja ReplicaSelector](#partition_replica_selector).


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector ja ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector on tarjoamia testattavuus, ja voit valita tietyn osio, johon haluat tehdä jonkin seuraavista testattavuus käytetään. Sen avulla voidaan valita tietyn osion, jos osion tunnuksen tunnetaan etukäteen. Tai voit antaa osio-näppäintä ja toiminnon ratkaisee osion tunnuksen sisäisesti. Käytössä on myös valita satunnaisia osio.

Apuohjelma käyttöön Luo PartitionSelector-objekti ja valitse osio ja käytä jotakin seuraavista menetelmistä Select *. Valitse Välitä PartitionSelector-objekti, joka edellyttää, että ohjelmointirajapinnan. Jos ei-vaihtoehto on valittuna, oletusarvoisesti satunnaisia osio.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector on tarjoamia testattavuus, ja valitse replika, jossa voit tehdä jonkin seuraavista testattavuus avulla. Sen avulla voidaan valita tietyn Jos replikan tunnus tunnetaan etukäteen. Lisäksi on valita ensisijainen replikan tai satunnaisia toissijainen. ReplicaSelector johdetaan PartitionSelector, joten valitse se ja osio, joina, jonka haluat suorittaa testattavuus-toimintoa.

Apuohjelma käyttöön ReplicaSelector objektin luominen ja määrittäminen niin kuin haluat, valitse se ja osio. Voit siirtää sen sitten Ohjelmointirajapinnan, joka edellyttää, että kyselyjä. Jos ei-vaihtoehto on valittuna, oletusarvoisesti satunnaisia replikan ja satunnaisia osio.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Seuraavat vaiheet

- [Testattavuus skenaariot](service-fabric-testability-scenarios.md)
- Palvelun testaaminen
   - [Virheiden simuloida palvelun työmääriä aikana](service-fabric-testability-workload-tests.md)
   - [Palvelun viestintä-virheet](service-fabric-testability-scenarios-service-communication.md)
