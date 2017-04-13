<properties
   pageTitle="Voit kutsua tietojen menettämisen palvelun kangasta Services | Microsoft Azure"
   description="Kerrotaan, miten voit käyttää tietojen menettämisen ohjelmointirajapinta"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Voit kutsua tietojen menettämisen Services

>[AZURE.WARNING] Tässä asiakirjassa kerrotaan, kuinka voit aiheuttaa tietojen menettämisen palvelujen ja kannattaa käyttää harkiten.

## <a name="introduction"></a>Johdanto
Voit kutsua tietojen menettämisen palvelun kangasta palvelun puheluja StartPartitionDataLossAsync() mukaan-osioon.  Tämä api käyttää vika lisäämisen ja analyysi-palvelun tekemässä töitä aiheuttaa tietojen menetyksen ehdot.

## <a name="using-the-fault-injection-and-analysis-service"></a>Vian lisäämisen ja analyysi-palvelun avulla

Vian lisäämisen ja analyysi-palvelu tukee tällä hetkellä seuraavia ohjelmointirajapinnan alla olevassa kaaviossa.  Kaavion oikealla puolella näkyy vastaavan PowerShell cmdlet-komennon.  Lue lisätietoja yksitellen kullekin API msdn-ohjeista.

|           C#-OHJELMOINTIRAJAPINTA                    |         PowerShell cmdlet-komento                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Käynnistä ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Käynnistä ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Käynnistä ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Yleistä komennon suorittaminen

Vian lisäämisen ja analyysi-palvelua käyttävät asynkroninen malli, jossa Vie komento yhden API, jäljempänä tässä asiakirjassa "Käynnistä"-Ohjelmointirajapinnan paina Tämä komento "GetProgress" Ohjelmointirajapinnan käyttäminen, kunnes se on muutettu pääte tilaan tai peruuttaa sen etenemistä tarkastaa.
Aloita komennon Ota "Käynnistä" Ohjelmointirajapinnan vastaavan Ohjelmointirajapinnan.  Tämä API palauttaa milloin vika lisäämisen ja analyysi-palvelu on hyväksynyt pyynnön.  Se ei tarkoita, kuinka paljon komento on suoritettu, tai vaikka se vielä aloitettu.  Tarkista edistyminen-komennon, jotta voit soittaa "GetProgress", joka vastaa aikaisemmalta nimeltä "Käynnistä" Ohjelmointirajapinnan API.  "GetProgress" API palauttaa objektin, joka ilmaisee, että sen tila-ominaisuuden sisällä komento nykyisen tilan.  Jatkuvasti, kunnes suorittaa komennon:

1.  Se onnistuu.  Jos olet soittanut "GetProgress" sitä tällöin, edistymisen objektin tila on valmis.
2.  Se tapahtuu vakava virhe.  Jos olet soittanut "GetProgress" sitä tällöin, edistymisen objektin tila virhe
3.  Peruuta kautta [CancelTestCommandAsync]  [ cancel] API tai [Pysäytä ServiceFabricTestCommand]  [ cancelps] PowerShell cmdlet-komennon.  Jos olet soittanut "GetProgress" sitä tällöin, edistymisen objektin tila on peruutettu tai ForceCancelled, kyseisen API-argumentin mukaan.  Ohjeissa [CancelTestCommandAsync]  [ cancel] lisätietoja.


## <a name="details-of-running-a-command"></a>Tietoja komennon suorittaminen

Jotta voit aloittaa komennon, Soita Käynnistä Ohjelmointirajapinnan odotettu argumentteja.  Kaikki Käynnistä ohjelmointirajapinnan on nimeltä toimintotunnukseksi GUID-tunnus-argumentti.  Sinun pitäisi seurantaan toimintotunnukseksi-argumentin jälkeen, kun sitä käytetään komennon edistymisen seuraaminen.  Tämä on siirrettävä kyselyjä "GetProgress" API edistymisen-komennon.  Toimintotunnukseksi on oltava yksilöllinen.

Sen jälkeen puhelut onnistuneesti Käynnistä-Ohjelmointirajapinta, GetProgress Ohjelmointirajapinnan on kutsuttava silmukassa ennen kuin palautetut edistymisen objektin tila-ominaisuuden on valmis.  Kaikki [käyttäjän FabricTransientException]  [ fte] ja OperationCanceledException's on yritettävä.
Kun komento on saavutettu pääte tila (valmis, Faulted tai peruutettu), palautettu edistymisen objektin tulos-ominaisuuden on lisätietoja.  Jos siinä on valmis, Result.SelectedPartition.PartitionId sisältää osion tunnuksen, joka on valittuna.  Result.Exception arvo on nolla.  Jos siinä on virhe, Result.Exception on syytä vika lisäämisen ja analyysi-palvelun virhe komento.  Result.SelectedPartition.PartitionId on osion tunnuksen, joka on valittuna.  Joissakin tilanteissa komento saattaa ei olet edennyt riittävän kaukana valitsemaan osio.  Siinä tapauksessa PartitionId on 0.  Jos Peruutettu tilaan, Result.Exception on null.  Faulted kirjainkoko, kuten Result.SelectedPartition.PartitionId on osion tunnuksen, joka on valittu, mutta jos komento ei ole on edennyt riittävän kaukana kiellä, se on 0.  Voit myös katsoa alla otosten.

Seuraava esimerkki koodi näytetään, miten Käynnistä sitten Tarkista edistyminen-komento, joka aiheuttaa tietojen menettämisen tiettyyn osioon.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Alla olevassa esimerkissä käyttämisestä PartitionSelector Valitse satunnainen osio määritetyn palvelun:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Historia ja katkaisu

Komento on muutettu pääte tilan, kun sen metatietoja säilyy tiettyyn aikaan, ennen kuin se poistetaan säästämiseksi vika lisäämisen ja analyysi-palvelu.  Jos "GetProgress" kutsutaan käyttämällä komennon toimintotunnukseksi, kun se on poistettu, se palauttaa FabricException virhekoodi KeyNotFound kanssa.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
