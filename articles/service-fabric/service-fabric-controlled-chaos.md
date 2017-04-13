<properties
   pageTitle="Aiheuttaa Chaos palvelun kangasta klustereissa | Microsoft Azure"
   description="Hallinta Chaos klusterin vika lisäämisen ja klusterin Analysis Service API avulla."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Aiheuttaa valvottu Chaos palvelun kangasta klustereissa
Suurissa jaettu järjestelmien, kuten cloud infrastruktuurin ovat salliminen epäluotettavista. Azure palvelun kangasta kehittäjät voivat kirjoittaa luotettavia palveluja epäluotettavista infrastruktuurin päälle. Tehokkaat services kirjoittamaan kehittäjät on voi aiheuttaa virheitä vastaan tämän epäluotettavista infrastruktuurin Testaa niiden palveluiden vakavuus.

Vian lisäämisen ja klusterin Analysis Service (tunnetaan myös nimellä vika analyysi palvelu) antaa kehittäjille voi aiheuttaa vika toiminnot Testaa palvelut. Kuitenkin kohdennettujen Simuloitu virheitä pääset vain tähän mennessä. Jos haluat ottaa testaus edelleen, voit käyttää Chaos.

Chaos varmistaminen jatkuva, limitetty virheitä (kaksivaiheista ja äkillisen) koko klusterin pitkän ajan päälle. Kun olet määrittänyt Chaos korvauksen ja virheet tyyppi, voit käynnistää tai pysäyttää se C#-ohjelmointirajapinnan tai PowerShell luomiseen virheitä klusterin ja palvelun kautta.

Chaos ollessa käynnissä, se tuottaa eri tapahtumat, jotka siepata ajankohtana, suorita tilan. ExecutingFaultsEvent sisältää esimerkiksi kaikki, joka suoritetaan, iteraatio-virheitä. ValidationFailedEvent sisältää tiedot, jotka klusterin vahvistuksen aikana löytyi epäonnistui. Voit kutsua GetChaosReportAsync Ohjelmointirajapinnan saadaksesi Chaos suoritetaan raportti.

## <a name="faults-induced-in-chaos"></a>Chaos aiheuttaa virheitä
Chaos Luo virheitä yli koko palvelun kangasta klusterin ja pakkaa virheitä, jotka näkyvät kuukausina tai vuosina muutaman tunneiksi. Suuri vika nykyisellä limitetty virheitä yhdistelmä löytää yläkulmassa palvelupyynnöt, jotka muuten vastattu. Tämä Chaos Harjoitus ohjaa palvelun laatua koodin merkittäviä parantamiseen.

Chaos aiheuttaa virheitä seuraavista ryhmistä:

 - Käynnistä solmu
 - Käynnistä uudelleen käyttöön koodi-paketti
 - Poista replika
 - Käynnistä replikan
 - Siirrä ensisijainen replikan (määritettäviä)
 - Siirrä toissijainen replikan (määritettäviä)

Chaos suorittaa useita toistoja. Iteraation koostuu virheitä ja ajanjakson klusterin vahvistus. Voit määrittää tasoittumista klusterin ja kelpoisuuden onnistuu aikaa. Jos virheen löytyy klusterin kelpoisuuden, Chaos Luo ja jatkuu ValidationFailedEvent UTC-aikaleima ja virheen tiedot.

Esimerkkinä Chaos, joka on määritetty toimimaan enintään kolme samanaikainen virheitä tuntia esiintymä. Chaos aiheuttaa kolme virheitä ja vahvistaa klusterin kunto. Se iteroivaa – edellinen vaihe, kunnes se on nimenomaisesti pysäytetty StopChaosAsync API-Liittymän kautta tai tunnin välittää. Jos klusterin tulee perusasemassa minkä tahansa iteraatio (eli se ei ole tasapainota määritetyn ajan kuluessa), Chaos Luo ValidationFailedEvent. Tapahtuman ilmaisee, että järjestelmässä on suoriutunut virhe on ehkä edelleen tutkimuksen.

Nykyisessä muodossaan Chaos aiheuttaa vain turvallisten virheitä. Tämä tarkoittaa sitä, että ulkoinen virheitä puuttuessa quorum menetys tai tietojen menettämistä ei koskaan esiintyisi.

## <a name="important-configuration-options"></a>Tärkeää kokoonpanoasetusten määrittäminen
 - **TimeToRun**: kokonaisaika, Chaos suoritetaan ennen kuin se päättyy onnistui. Voit lopettaa Chaos ennen kuin se on suoritettu TimeToRun kauden StopChaos API-Liittymän kautta.
 - **MaxClusterStabilizationTimeout**: odotusaika luomisella kunnossa tarkistuksen sen uudelleen ennen klusterin enimmäismäärän. Tämä odottaa on lyhentää klusterin kuormitus samalla, kun se on palautetaan. Suorittaa tarkistukset ovat seuraavat:
    - Jos klusterin kunto on OK
    - Jos palvelun kunto on OK
    - Jos kohde se koon määrittäminen saavutetaan service-osio
    - InBuild replikoita ei ole olemassa
 - **MaxConcurrentFaults**: samanaikainen virheitä, jotka ovat aiheuttaa iteraation enimmäismäärä. Mitä suurempi luku, mitä enemmän kohdetoimialueen Chaos on. Tuloksena on monimutkaisempi failovers ja siirtymä yhdistelmät. Chaos takaa, että ulkoinen virheitä puuttuessa ei ole quorum tappio tai tietojen menettämistä, riippumatta siitä, kuinka suuren arvon määritysten on.
 - **EnableMoveReplicaFaults**: ottaa käyttöön tai poistaa käytöstä, voit siirtää ensisijaisen tai toissijaisen-replikoida aiheuttaa virheitä. Nämä virheet on oletusarvoisesti poistettu käytöstä.
 - **WaitTimeBetweenIterations**: ajan odottamaan välillä, eli PYÖRISTÄ-funktiota virheitä ja vastaavan vahvistuksen jälkeen.
 - **WaitTimeBetweenFaults**: kaksi peräkkäistä virheet-iteraation välisen ajan.

## <a name="how-to-run-chaos"></a>Chaos suorittaminen
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShellin:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
