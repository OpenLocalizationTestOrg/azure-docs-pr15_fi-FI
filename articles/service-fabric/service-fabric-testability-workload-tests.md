<properties
   pageTitle="Mukautetun testi skenaariot | Microsoft Azure"
   description="Kuinka vastaan kaksivaiheista ja äkillisen palvelujen tiukentamista."
   services="service-fabric"
   documentationCenter=".net"
   authors="anmolah"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="anmola"/>

# <a name="simulate-failures-during-service-workloads"></a>Virheiden simuloida palvelun työmääriä aikana

Azure-palvelu kangasta testattavuus skenaariot avulla kehittäjät voivat huolehtia ei myyminen yksittäisiä virheitä. On tilanteissa, jossa eksplisiittinen välilehdet asiakkaan kuormituksen ja virheet voi olla tarpeen. Asiakkaan kuormituksen ja virheet välilehdet varmistaa, että palvelu on suoritat jonkin toiminnon kun tapahtuu virhe. Valita ohjausobjektin, joka sisältää testattavuus taso, nämä voi olla työmäärää suorittamisen tarkka kohtiin. Tämä virheitä tilat-sovelluksen osoitteessa induction etsiä virheet ja parantaa laatua.

## <a name="sample-custom-scenario"></a>Mukautetun esimerkkitilanne
Tämän testin näkyy tilanne, jossa interleaves business kuormituksen [kaksivaiheista ja äkillisen](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions)kanssa. Virheitä pitäisi aiheuttaa palvelun toimintojen tai Laske saat parhaan tuloksen keskellä.

Oletetaan, että käydä läpi palvelu, joka paljastaa neljä työmääriä Esimerkki: A, B, C ja d vastaa joukko työnkulut ja voi olla suorittaminen ja tallennustilaa pitämällä järjestelmässä. Yksinkertaisuuden emme abstrakti työtaakkaa, tässä esimerkissä. Tässä esimerkissä suoritetaan eri virheitä ovat seuraavat:
  + RestartNode: Äkillisen vika, jotta saadaan aikaan tietokoneen uudelleenkäynnistys.
  + RestartDeployedCodePackage: Äkillisen vika, jotta saadaan aikaan palvelun host-prosessi kaatuu.
  + RemoveReplica: Kaksivaiheista vika voit simuloida replikan poistamista.
  + MovePrimary: Kaksivaiheista vika, jotta saadaan aikaan replikan siirtää palvelun kangasta kuormituksen käynnistämä.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
