<properties
   pageTitle="Azure-palvelu kangasta kohteiden tarkasteleminen koostetun kunto | Microsoft Azure"
   description="Kysely, tarkastella ja arvioida Azure palvelun kangasta kohteiden koostetun kunto kautta kunto kyselyjen Yleiset kyselyjen ja käsitellään."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="view-service-fabric-health-reports"></a>Näytä palvelun kangasta kuntoraportit
Azure palvelun kangasta esitellään [kunto-malli](service-fabric-health-introduction.md) , joka koostuu kunto kohteiden mitä järjestelmän osat ja watchdogs voit raportoida paikallisen tilanteita, joissa ne seurantaa. [Kuntotietojen tallentaa](service-fabric-health-introduction.md#health-store) kokoaa yhteen kaikki sen selvittämisestä, onko kohteiden kunnossa kunto tiedot.

Klusterin lisätään ulos-ruutuun lähetetty järjestelmän osien kuntoraportit. Lisätietoja on osoitteessa [kunto raporttien vianmääritys](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Palvelun kangasta tarjoaa useita tapoja siirtyä kohteiden koostetun kunto:

- [Palvelun kangasta Explorer](service-fabric-visualizing-your-cluster.md) tai muita visualisoinnin työkaluja

- Kuntotietojen kyselyt (kautta PowerShell Ohjelmointirajapinnan ja muut)

- Yleiset kyselyt, joka palauttaa luettelon kohteet, jotka sisältävät kunto kuin yksi ominaisuuksista (kautta PowerShell Ohjelmointirajapinnan ja muut)

Nämä asetukset osoittamaan käyttää paikallisen klusterin viisi solmujen kanssa. Vieressä **kangasta: / järjestelmän** sovelluksen (joka on olemassa ruutuun ulos)-sovelluksia on otettu käyttöön. Nämä sovellukset on **kangasta: / WordCount**. Tämän sovelluksen sisältää tilallisten palvelu, joka on määritetty seitsemän replikoiden kanssa. Koska vain viisi solmujen järjestelmäosat Näytä varoitusta osio on pienempi kohde määrä.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Kuntotietojen kangasta Explorerissa
Palvelun kangasta Explorer sisältää klusterin visuaalisen näkymän. Alla olevassa kuvassa näet:

- Sovelluksen **kangasta: / WordCount** on punainen (-virhe), koska se on virhe tapahtuman ilmoittaa **MyWatchdog** ominaisuuden **saatavuus**.

- Jokin seuraavista sen palvelujen **kangasta: / WordCount/WordCountService** keltainen (-varoitus). Palvelu on määritetty seitsemän replikoiden kanssa, koska ne et voi kaikki on sijoitettu, on vain viisi solmujen. Vaikka se ei näy tässä, service-osio on keltainen järjestelmän raportin vuoksi. Keltainen osion käynnistää keltainen palvelu.

- Klusterin on punainen punainen sovelluksen vuoksi.

Laskennan käyttää oletuskäytäntöjä klusterin luettelo ja sovellusluettelo. Ne tarkka käytännöt ja Hyväksy kaikki.

Klusterin kangasta Resurssienhallinnassa näkymä:

![Näytä klusterin kangasta Resurssienhallinnassa.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Lisätietoja [Palvelun kangasta Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Kunto-kyselyt
Palvelun kangasta paljastaa kunto kyselyjen kunkin tuetuista [kohde tiedostotyypit](service-fabric-health-introduction.md#health-entities-and-hierarchy). Niitä voi käyttää API (menetelmiä löytyy **FabricClient.HealthManager**), PowerShellin cmdlet-komennot ja muille käyttäjille. Nämä kyselyt palauttaa kohteen valmis kunto tietoja: koostetun kuntotietojen, kohteen kunto tapahtumia, lapsen kunto hyötyä (tarvittaessa) ja perusasemassa arvioinnit, kun kohde ei ole kunnossa.

> [AZURE.NOTE] Kuntotietojen kohde palautetaan, kun se lisätään täysin kunto-kaupan. Kohde on on aktiivinen (ei poisteta) ja järjestelmä-raportti. Sen ylemmän tason kohteita, valitse hierarkian ketju on myös oltava Järjestelmäraportit. Jos jokin nämä ehdot eivät täyty, kunto-kyselyt palauttaa poikkeuksen, joka osoittaa, miksi kohde ei palautetaan.

Kunto-kyselyt on välitettävä kohteen tunnus, joka määräytyy kohteen tyyppi. Kyselyjen Hyväksy valinnainen kunto parametrit. Jos ole kunto-käytäntöjä ei määritetä,-klusterin tai sovelluksen luettelo [kuntokäytännöt](service-fabric-health-introduction.md#health-policies) käytetään arviointia varten. Kyselyjen Hyväksy myös suodattimia, joka palauttaa vain osittaisen lasten tai events: niistä, jotka noudattavat määritetyillä suodattimilla.

> [AZURE.NOTE] Tulostus-suodattimet ovat käytössä palvelinpuolen, jotta viestin vastaus on pienennettynä. On suositeltavaa, että voit rajoittaa tietoja tulosteen suodattimien avulla sijaan asiakaspuolen suodattimia.

Yksikön kunto sisältää:

- Kohteen koostetun kuntotietojen. Laskettu kunto säilön kohteen kuntoraportit, lapsen kunto hyötyä (tarvittaessa) ja terveyteen käytännöt perusteella. Lisätietoja [kohteen kunto arviointi](service-fabric-health-introduction.md#entity-health-evaluation).  

- Yksikön kunto-tapahtumat.

- Sivustokokoelman kaikki objektit, joka voi olla lasten lasten valtion kunto. Kunto-tilan sisältävät kohteen tunnisteet ja koostetut kuntotietojen tarkistus. Pääset valmis kunto lapselle Ota kyselyn kuntotietojen alatason kohteen tyyppi ja välittää ali-tunniste.

- Perusasemassa arvioinnit, osoita raportin, joka on käynnistänyt kokonaisuus, tila, jos kohde ei ole kunnossa.

## <a name="get-cluster-health"></a>Hae klusterin kunto
Palauttaa klusterin kohteen kuntotietojen ja kuntotietojen tiloista sovellukset ja solmujen (lapset klusterin) sisältää. IME:

- [Valinnainen] Klusterin-kunto käytäntö arvioida solmut ja klusterin tapahtumat.

- [Valinnainen] Sovelluksen kunto käytännön kartassa kanssa käytettävä sovelluksen tiedostojen käytännöt ohittavat kuntokäytännöt.

- [Valinnainen] Tapahtumien, solmujen ja sovelluksia, jotka määrittävät, mitkä tapahtumat suodattimet kiinnostavat ja palautetaan tuloksen (esimerkiksi vain-virheet tai varoitukset ja virheet). Kaikki tapahtumat, solmujen ja sovellusten käytetään kohteen koostetun kunnon, riippumatta siitä, suodatin.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Saat klusterin kunto-luominen `FabricClient` ja soittaminen sen **HealthManager** [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) -menetelmää.

Seuraava kutsu saa klusterin kuntotietojen:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Seuraava koodi saa klusterin kuntotietojen käyttämällä mukautettuja klusterin kunto-käytännön luominen ja suodattimet solmujen ja sovellukset. Se luo [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), jossa on syötteen tietoja.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShellin
Saat klusterin kuntotietojen cmdlet-komento on [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla.

Klusterin tila on viisi solmujen, järjestelmäsovelluksen ja kangasta: / WordCount määritetty kuvatulla tavalla.

Seuraavan cmdlet-komennon saa klusterin kunto kunto oletuskäytäntöjä avulla. Koostetun kuntotietojen tarkistus on varoitus, koska kangasta: / WordCount-sovellus on varoitus. Huomaa, miten perusasemassa arvioinnit lisätiedot, joka on käynnistänyt koostetun kunto edellytyksistä.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Seuraavan PowerShell cmdlet-komennon saa klusterin kunto mukautetun sovelluksen käytännön avulla. Se suodattaa vain virhe tai varoitus sovellukset ja solmujen tulokset. Tämän vuoksi ei ole solmujen palautetaan sellaisina kuin ne ovat kaikki kunnossa. Vain kangasta: / WordCount sovelluksen kunnioitetaan sovellusten suodatin. Koska varoitukset virheinä kangasta, sinun kannattaa määrittää mukautetun käytännön: / WordCount sovelluksen sovelluksen arvioidaan, kuten virhe ja niin on klusterin.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat klusterin kunto [GET-pyynnöt](https://msdn.microsoft.com/library/azure/dn707669.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/dn707696.aspx) , joka sisältää kuntokäytännöt on kuvattu tekstissä.

## <a name="get-node-health"></a>Hae solmu kunto
Palauttaa solmu kohteen kunto ja sisältää raportoitu solmun kunto-tapahtumat. IME:

- [Pakollinen] Solmunimi, joka määrittää solmun.

- [Valinnainen] Klusterin kunto käytäntöasetukset arvioida kunto.

- [Valinnainen] Tapahtumien, joka määrittää, mitkä tapahtumat kiinnostavat ja palautetaan tuloksen (esimerkiksi vain-virheet tai varoitukset ja virheet) suodattimet. Kaikki tapahtumat käytetään kohteen koostetun kunnon, riippumatta siitä, suodatin.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Tulee solmu kunto Ohjelmointirajapinnan, luo `FabricClient` ja soittaminen sen HealthManager [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) -menetelmää.

Seuraava koodi saa solmu kuntotietojen määritetty solmu nimi:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Seuraava koodi noutaa solmu kuntotietojen määritetyn Solmunimi ja tapahtumien suodatus-ja mukautettu käytäntö kulkee [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShellin
Saat solmu kuntotietojen cmdlet-komento on [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla.
Seuraavan cmdlet-komennon saa solmu kuntotietojen kunto oletuskäytäntöjä avulla:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Seuraavan cmdlet-komennon saa kaikkien solmujen kunto klusterin:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat solmu kunto [GET-pyynnöt](https://msdn.microsoft.com/library/azure/dn707650.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/dn707665.aspx) , joka sisältää kuntokäytännöt on kuvattu tekstissä.

## <a name="get-application-health"></a>Hanki sovellus kunto
Palauttaa kohteen sovelluksen kunto. Se sisältää sovelluksen ja palvelun alisivustojen käyttöä rajoitetaan kunto tiloista. IME:

- [Pakollinen] Sovelluksen nimi (URI), joka määrittää sovelluksen.

- [Valinnainen] Sovelluksen-kuntokäytännön avulla sovelluksen tiedostojen käytännöt ohitetaan.

- [Valinnainen] Tapahtumien, palvelujen ja otettu käyttöön sovellukset, jotka määrittävät, mitkä tapahtumat suodattimet kiinnostavat ja palautetaan tuloksen (esimerkiksi vain-virheet tai varoitukset ja virheet). Kaikki tapahtumat, palvelujen ja otettu käyttöön sovellusten käytetään kohteen koostetun kunnon, riippumatta siitä, suodatin.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Saat sovelluksen kunto-luominen `FabricClient` ja soittaminen sen HealthManager [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) -menetelmää.

Seuraava koodi saa sovelluksen kuntotietojen määritetyn sovelluksen nimen (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Seuraava koodi saa sovelluksen kuntotietojen määritetty sovelluksen nimi (URI), suodattimet ja mukautetut käytännöt, jotka on määritetty [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx)kautta.

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShellin
Saat sovelluksen kuntotietojen cmdlet-komento on [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla.

Seuraavat cmdlet-komento palauttaa kunto **kangasta: / WordCount** sovelluksen:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Seuraavan PowerShell cmdlet-komennon välittää mukautetut käytännöt. Se suodattaa myös lasten ja tapahtumia.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat sovelluksen kunto [GET-pyynnöt](https://msdn.microsoft.com/library/azure/dn707681.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/dn707643.aspx) , joka sisältää kuntokäytännöt on kuvattu tekstissä.

## <a name="get-service-health"></a>Hae palvelun kunto
Palauttaa palvelu kohteen kunto. Se sisältää osion kunto-tilan. IME:

- [Pakollinen] Palvelunimi (URI), joka määrittää palvelu.

- [Valinnainen] Sovelluksen-kunto käytäntö ohittaa sovelluksen tiedostojen käytännön avulla.

- [Valinnainen] Tapahtumien ja osioita, joka määrittää, mitkä tapahtumat kiinnostavat ja palautetaan tuloksen (esimerkiksi vain-virheet tai varoitukset ja virheet) suodattimet. Kaikki tapahtumat ja osioiden käytetään kohteen koostetun kunnon, riippumatta siitä, suodatin.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Tulee palvelun kunto Ohjelmointirajapinta, luo `FabricClient` ja soittaminen sen HealthManager [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) -menetelmää.

Seuraava esimerkki noutaa määritetyn palvelun nimellä (URI) palvelun kunto:

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Seuraava koodi saa palvelun nimen (URI)-palvelun kunto suodattimet ja mukautetun käytännön [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx)kautta:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShellin
Saat palvelun kunto cmdlet-komento on [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla.

Seuraavan cmdlet-komennon saa palvelun kunto kunto oletuskäytäntöjä avulla:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat palvelun kunto [GET-pyynnöt](https://msdn.microsoft.com/library/azure/dn707609.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/dn707646.aspx) , joka sisältää kuntokäytännöt on kuvattu tekstissä.

## <a name="get-partition-health"></a>Hae osion kunto
Palauttaa osiota kohteen kunto. Se sisältää replikan kunto-tilan. IME:

- [Pakollinen] Osion tunnus (GUID), joka määrittää osio.

- [Valinnainen] Sovelluksen-kunto käytäntö ohittaa sovelluksen tiedostojen käytännön avulla.

- [Valinnainen] Tapahtumien ja replikat, joka määrittää, mitkä tapahtumat kiinnostavat ja palautetaan tuloksen (esimerkiksi vain-virheet tai varoitukset ja virheet) suodattimet. Kaikki tapahtumat ja replikoiden käytetään kohteen koostetun kunnon, riippumatta siitä, suodatin.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Tulee osion kunto Ohjelmointirajapinnan, luo `FabricClient` ja soittaminen sen HealthManager [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) -menetelmää. Määritä valinnaisten parametrien [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx)luominen

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShellin
Saat osion kuntotietojen cmdlet-komento on [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla.

Seuraavan cmdlet-komennon saa kaikki osiot terveys **kangasta: / WordCount/WordCountService** palvelu:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat osion kunto [GET-pyynnöt](https://msdn.microsoft.com/library/azure/dn707683.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/dn707680.aspx) , joka sisältää kuntokäytännöt on kuvattu tekstissä.

## <a name="get-replica-health"></a>Hae replikan kunto
Palauttaa tilallisten palvelun replikan tai tilattomien palvelun esiintymän kunto. IME:

- [Pakollinen] Osion GUID-tunnus ja replikan tunnus, joka määrittää se.

- [Valinnainen] Sovelluksen kunto-käytäntöparametrit käytettävä sovelluksen tiedostojen käytännöt ohitetaan.

- [Valinnainen] Tapahtumien, joka määrittää, mitkä tapahtumat kiinnostavat ja palautetaan tuloksen (esimerkiksi vain-virheet tai varoitukset ja virheet) suodattimet. Kaikki tapahtumat käytetään kohteen koostetun kunnon, riippumatta siitä, suodatin.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Tulee replikan kuntotietojen Ohjelmointirajapinnan, luo `FabricClient` ja soittaminen sen HealthManager [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) -menetelmää. Määritä lisäasetukset parametrit käyttämällä [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShellin
Saat replikan kuntotietojen cmdlet-komento on [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla.

Ensisijainen se kaikki osiot palvelun kunto-saa seuraavan cmdlet-komennon:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat replikan kunto [GET-pyynnöt](https://msdn.microsoft.com/library/azure/dn707673.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/dn707641.aspx) , joka sisältää kuntokäytännöt on kuvattu tekstissä.

## <a name="get-deployed-application-health"></a>Hae sovelluksen kunto
Palauttaa sovelluksen käyttöön solmu kohteen kunto. Se on otettu käyttöön palvelun pakettien kunto tilat. IME:

- [Pakollinen] Sovelluksen nimi (URI) ja Solmunimi (merkkijono), joka tunnistaa sovelluksen.

- [Valinnainen] Sovelluksen-kuntokäytännön avulla sovelluksen tiedostojen käytännöt ohitetaan.

- [Valinnainen] Tapahtumien ja otettu käyttöön palvelun-paketteja, jotka määrittävät, mitkä tapahtumat suodattimet kiinnostavat ja palautetaan tuloksen (esimerkiksi vain-virheet tai varoitukset ja virheet). Kaikki tapahtumat ja otettu käyttöön palvelupakettien käytetään kohde yhdistetään kunnon, riippumatta siitä, suodatin.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Saat sovelluksen käyttöön Ohjelmointirajapinnan kautta solmu kunto-luominen `FabricClient` ja soittaminen sen HealthManager [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) -menetelmää. Määritä valinnaisten parametrien käyttämällä [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShellin
Saat sovelluksen kuntotietojen cmdlet-komento on [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla. Saat selville, jossa sovellus on otettu käyttöön, suorittamalla [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) ja katsomalla sovelluksen lasten.

Seuraavan cmdlet-komennon saa kunto **kangasta: / WordCount** sovelluksen käyttöön **_Node_2**.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat sovelluksen kunto [GET-pyynnöt](https://msdn.microsoft.com/library/azure/dn707644.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/dn707688.aspx) , joka sisältää kuntokäytännöt on kuvattu tekstissä.

## <a name="get-deployed-service-package-health"></a>Hae käyttöön palvelun package kunto
Palauttaa käyttöön palvelu-paketti-kohteen kunto. IME:

- [Pakollinen] Sovelluksen nimi (URI), Solmunimi (merkkijono) ja palvelun luettelon nimi (merkkijono), joka tunnistaa käyttöön palvelupakettiin.

- [Valinnainen] Sovelluksen-kunto käytäntö ohittaa sovelluksen tiedostojen käytännön avulla.

- [Valinnainen] Tapahtumien, joka määrittää, mitkä tapahtumat kiinnostavat ja palautetaan tuloksen (esimerkiksi vain-virheet tai varoitukset ja virheet) suodattimet. Kaikki tapahtumat käytetään kohteen koostetun kunnon, riippumatta siitä, suodatin.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Tulee käyttöön palvelupakettiin kunto Ohjelmointirajapinnan, luo `FabricClient` ja soittaminen sen HealthManager [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) -menetelmää. Määritä valinnaisten parametrien käyttämällä [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShellin
Saat käyttöön palvelun paketin kunto cmdlet-komento on [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla. Jos haluat nähdä, jossa sovellus on otettu käyttöön, suorittamalla [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) ja katsomalla käyttöön sovellukset. Mitä palvelun paketit ovat sovelluksen näkyviin Tarkista käyttöön palvelun package lasten [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) tulosteessa.

Seuraavan cmdlet-komennon saa **WordCountServicePkg** palvelupakettiin terveys **kangasta: / WordCount** sovelluksen käyttöön **_Node_2**. Kohteella **System.Hosting** raporttien onnistuneen palvelupakettiin ja aloituskohdan aktivointi ja onnistui-palvelutyyppi rekisteröinti.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat käyttöön palvelun paketin kunto [GET-pyynnöt](https://msdn.microsoft.com/library/azure/dn707677.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/dn707689.aspx) , joka sisältää kuntokäytännöt on kuvattu tekstissä.

## <a name="health-chunk-queries"></a>Kuntotietojen lohkon kyselyt
Kuntotietojen lohkon kyselyt voi palauttaa kohti syötteen suodattimet lasten monitasoisten klusterin (rekursiivisesti). Se tukee Lisäsuodattimet, joka mahdollistaa paljon joustavuutta express palautetaan, mitkä tietyn lasten merkittyä niiden yksilöllinen tai muut ryhmän tunnus ja/tai kuntotietojen tarkistus. Oletusarvon mukaan ei lasten paketti sisältää, kunto-komennot, jotka ovat aina ensimmäisen tason lasten sijaan.

[Kuntotietojen kyselyjen](service-fabric-view-entities-aggregated-health.md#health-queries) palauttaa vain ensimmäisen tason lasten määritetyn yrityksen tarvittavat suodattimet kohden. Saat lasten lapset-Kutsu muita kunto ohjelmointirajapinnan halutut kunkin kohteen. Saat tiettyjen kohteiden kunto-on vastaavasti Soita yhden kunto API kunkin haluamasi kohteen. Suodatuksen lisäasetukset lohko-kyselyn avulla voit pyytää halutut pienentäminen viestin koko ja viestien määrä kyselyssä useita kohteita.

Lohko-kyselyn arvo on, että voit noutaa kuntotietojen Lisää klusterin kohteiden (mahdollisesti klusterin kohteilla aloittaen tarvittavat pääkansion) yhden käynnissä. Voit express monimutkaisia kunto kyselyn seuraavasti:

- Palauttaa vain sovellukset, virhe ja näiden sovellusten sisällyttää kaikki palvelut osoitteessa varoitus | virhe. Palautetut Services Sisällytä kaikki osiot.

- Palauttaa vain heidän nimensä määrittämää 4 sovellusten kunto.

- Palauttaa vain haluamasi sovellus tyyppisiä sovelluksia kunto.

- Palauttaa kaikki käyttöön kohteiden solmu. Palauttaa kaikki sovellukset, kaikki käyttöön määritetty solmu-sovelluksia ja kaikki kyseisen solmun käyttöön palvelun paketit.

- Palauttaa kaikki replikat virheen. Palauttaa kaikki sovellukset, palvelut, osiot ja vain replikoiden virheen.

- Palauttaa kaikki sovellukset. Tietyn palvelun Sisällytä kaikki osiot.

Tällä hetkellä kunto lohkon kyselyn näkyvät vain klusterin kohteen. Se palauttaa klusterin kunto lohkossa, joka sisältää:

- Klusterin koostetun kuntotietojen tarkistus.

- Solmujen, jotka noudattavat syötteen suodattimet kunto tilan lohko-luettelosta.

- Kuntotietojen tilan lohkon luettelo sovelluksia, jotka noudattavat syötteen suodattimet. Jokainen sovelluksen kunto tilan lohko sisältää lohko-luettelosta kaikki palvelut, jotka noudattavat syötteen suodattimet ja lohkon luettelon kaikkien käyttöön sovellukset, jotka noudattavat suodattimet. Sama lasten palveluiden ja otettu käyttöön sovellukset. Tällä tavalla kaikkien kohteiden klusterin voidaan mahdollisesti palauttaa pyydettäessä jäsennettävällä.

### <a name="cluster-health-chunk-query"></a>Klusterin kunto lohkon kysely
Palauttaa klusterin kohteen kunto ja sisältää tarvittavat lasten hierarkkisia kunto-tilan näkyvissä. IME:

- [Valinnainen] Klusterin-kunto käytäntö arvioida solmut ja klusterin tapahtumat.

- [Valinnainen] Sovelluksen kunto käytännön kartassa kanssa käytettävä sovelluksen tiedostojen käytännöt ohittavat kuntokäytännöt.

- [Valinnainen] Solmujen ja sovelluksia, jotka määrittävät, mitkä tapahtumat kiinnostavat ja palautetaan tuloksen suodattimet. Suodattimet ovat kohteen/ryhmään kohteiden tai kaikki samalla tasolla oleviin kohteisiin sovelletaan. Suodattimien luettelo voi olla yksi Yleiset suodatin ja/tai tietyn tunnisteille hieno syyt kohteisiin kysely palauttaa suodattimet. Jos se on tyhjä, lasten palautetaan ei oletusarvoisesti.
Lisätietoja suodattimien [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) ja [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). Sovelluksen suodattimet voivat rekursiivisesti Määritä erikoissuodattimia lasten.

Lohkon tulos on lasten, jotka noudattavat suodattimet.

Tällä hetkellä lohkon kysely ei palauta perusasemassa arvioinnit tai kohteen tapahtumat. Ylimääräisten tietojen saadaan aiemmin luodun klusterin kunto-kyselyn avulla.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Saat klusterin kunto lohko-luominen `FabricClient` ja soittaminen sen **HealthManager** [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) -menetelmää. Voit siirtää [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) kuvaamaan kuntokäytännöt- ja Erikoissuodattimet.

Seuraava koodi saa klusterin kunto lohkon Lisäsuodattimet kanssa.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShellin
Saat klusterin kuntotietojen cmdlet-komento on [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Ensin muodostaa yhteyttä klusterin [Yhdistä ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdlet-komennolla.

Seuraava koodi saa solmujen vain, jos ne eivät virhe lukuun ottamatta tietyn solmun, jotka aina palauttaa.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Seuraavan cmdlet-komennon saa klusterin lohkon sovelluksen suodattimilla.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

Seuraavat cmdlet-komento palauttaa käyttöön kohteilla solmu.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Saat klusterin kunto lohkon [GET-pyynnöt](https://msdn.microsoft.com/library/azure/mt656722.aspx) tai [Kirjaa pyynnön](https://msdn.microsoft.com/library/azure/mt656721.aspx) , joka sisältää kuntokäytännöt ja erikoissuodattimien, joka on kuvattu tekstissä.

## <a name="general-queries"></a>Yleiset kyselyt
Yleiset kyselyt palauttaa tietyntyyppisistä palvelun kangasta kohteiden luettelo. Ne näkyvät API (joko **FabricClient.QueryManager**menetelmiä), PowerShellin cmdlet-komennot ja muille käyttäjille. Nämä kyselyt koostaa alikyselyjen useita osia. Jonkin niistä on [kunto tallentaa](service-fabric-health-introduction.md#health-store), joka täyttää koostetun kuntotietojen jokainen kyselyn tulos.  

> [AZURE.NOTE] Yleistä kyselyiden palauttaa koostetun kuntotietojen kohteen ja RTF kuntoa tiedot eivät sisällä. Jos kohde ei ole kunnossa, voit seurata kunto kyselyjen kaikki sen kunto tiedot, kuten tapahtumat, lapsen kunto hyötyä ja perusasemassa arvioinnit.

Jos Yleiset kyselyt palauttavat Tuntematon kuntotietojen kohteen, on mahdollista kunto-kaupan ei ole täydellinen kohteen tietoja. On myös mahdollista alikyselyn kunto säilöön ei onnistu (esimerkiksi viestintä-virhe tai kunto-kaupan on rajoittanut). Seuranta-kohteen kunto kyselyn avulla. Jos alikyselyn lyhytkestoisia virheet, kuten verkko-ongelmien seuranta kyselyn välttämättä onnistu. Se voi myös antaa sinulle lisätietoja siitä, miksi kohde ei näy kunto-kaupasta.

Kyselyitä, jotka sisältävät **HealthState** kohteiden ovat seuraavat:

- Solmun luettelon: palauttaa luettelon solmut klusterin (sivutettu).
  - API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShellin: Get-ServiceFabricNode
- Sovellusluettelo: palauttaa (sivutettu) klusterin sovellusten luettelossa.
  - API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShellin: Get-ServiceFabricApplication
- Palveluluettelo: palauttaa palveluluetteloon (sivutettu)-sovelluksessa.
  - API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShellin: Get-ServiceFabricService
- Osion luettelo: palauttaa luettelo yhdistettävistä osioista palvelu (sivutettu).
  - API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShellin: Get-ServiceFabricPartition
- Replikan luettelon: palauttaa replikoiden luettelossa (sivutettu)-osiossa.
  - API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShellin: Get-ServiceFabricReplica
- Ottaa käyttöön sovellusluettelo: palauttaa solmu käyttöön sovellusten luettelossa.
  - API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShellin: Get-ServiceFabricDeployedApplication
- Käyttöön paketin Palveluluettelo: palauttaa palvelupakettien luettelo käyttöön-sovelluksessa.
  - API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShellin: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Jotkin kyselyt palauttaa sivutettu tulokset. Palaa kyselyistä on peräisin luettelo [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Jos tulokset eivät sovellu viestin, funktio palauttaa vain sivun ja ContinuationToken, jossa seurataan missä luettelointi pysäytetty. Jatka voit soittaa saman kyselyn ja välittää jatkumista tunnuksen saat seuraavan tuloksen edellisestä kyselystä.

### <a name="examples"></a>Esimerkkejä

Seuraava koodi saa perusasemassa sovellusten klusterin:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Seuraavan cmdlet-komennon noutaa hakemuksen tiedot kangasta: / WordCount sovelluksen. Huomaa, että kuntotietojen on varoitus.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Seuraavan cmdlet-komennon saa palvelut varoitus kuntotietojen kanssa:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Klusterin ja sovelluksen päivitykset
Klusterin ja sovelluksen valvottu päivityksen aikana palvelun kangasta tarkistaa, että kaikki pysyy kunnossa kunto. Jos kohde on perusasemassa kuin määritetty kunto käytäntöjen avulla lasketaan, päivitys koskee päivityksen kielikohtaiset käytännöt seuraavan toiminnon. Päivitys voi keskeyttää sallimaan käyttäjän toimia (kuten korjaaminen virhetilanteita tai muuttamalla käytännöt) tai sitä automaattisesti peruuttaa hyvä edellisen version.

*Klusterin* -päivityksen aikana saat klusterin päivityksen tila. Päivityksen tila on perusasemassa arviointitietoja, jotka osoittavat ominaisuudet perusasemassa klusterin. Jos päivitys peruutetaan vuoksi ongelmia, päivityksen tila muistaa viimeisen perusasemassa syistä. Nämä tiedot auttavat tutkia, mitä tapahtui virhe, kun päivitys on palautettu tai pysäyttää järjestelmänvalvojat.

Vastaavasti *sovelluksen* päivityksen aikana kaikki perusasemassa arvioinnit sisältyvät sovelluksen päivityksen tila.

Seuraavat näkyy muokattu kangasta sovelluksen päivityksen tila: / WordCount sovelluksen. Watchdog raportoi virheestä johonkin sen replikat. Päivitys on juoksevan takaisin, koska terveystarkastukset noudatetaan ei.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Lisätietoja [palvelun kangasta sovelluksen päivittäminen](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Käytä kunto arvioinnit vianmääritys
Aina, kun näkyvissä on klusterin tai sovelluksen ongelmasta, tarkista Miksei pinpoint klusteriin tai sovelluksen kunto. Perusasemassa arvioinnit tarkkoja tietoja mitä käynnistää nykyisen virheellisessä tilassa. Jos haluat, voit siirtyä alaspäin perusasemassa alatason kohteiden tunnistavan pääkansion syy kyselyjä.

> [AZURE.NOTE] Perusasemassa arvioinnit Näytä ensimmäinen syy kohteen arvioidaan nykyinen kunto-tilaan. Voi olla useita muita tapahtumia, jotka käynnistävät tässä tilassa, mutta ne ovat ei näytetä arvioinnit. Saat lisätietoja siirtyä alirakenteeseen perusasemassa raporttien klusterin laskemisesta kunto-kohteita.

## <a name="next-steps"></a>Seuraavat vaiheet
[Järjestelmän kunnon raporttien vianmääritys](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Lisää mukautettu palvelun kangasta kuntoraportit](service-fabric-report-health.md)

[Raportin ja tarkista palvelun kunto](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Valvo ja vianmäärityksen paikallisesti palvelut](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Palvelun kangasta sovelluksen päivitys](service-fabric-application-upgrade.md)
