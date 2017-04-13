<properties
   pageTitle="Järjestelmän kunto-raportteja sisältäviä vianmääritys | Microsoft Azure"
   description="Tässä artikkelissa kuvataan lähettämä Azure palvelun kangasta osat ja niiden käyttö vianmäärityksen klusterin tai sovelluksen ongelmien kuntoraportit."
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

# <a name="use-system-health-reports-to-troubleshoot"></a>Järjestelmän kunnon raporttien vianmääritys

Azure palvelun kangasta osien raportti ulos ruutuun klusterin kaikki kohteet. [Kuntotietojen tallentaa](service-fabric-health-introduction.md#health-store) Luo ja poistaa kohteita Järjestelmäraportit perusteella. Se myös järjestää ne hierarkiassa, joka kuvaa kohteen vuorovaikutukset.

> [AZURE.NOTE] Selvittääksesi terveyteen liittyvistä käsitteistä, Lue lisää osoitteessa [kangasta palvelun kunto-malli](service-fabric-health-introduction.md).

Järjestelmän kunnon raporteissa näkyvyys klusterin ja toiminnoista ja Merkitse ongelmat kunto kautta. Sovellusten ja palveluiden järjestelmän kuntoraportit varmistaa, että kohteita on otettu käyttöön, ja oikein palvelun kangasta näkökulmasta muunnettuna. Raporttien ei ole mitään kunto liiketoimintalogiikan palvelun tai valvonnan tunnistus jumittua prosesseista. Käyttäjäpalvelujen voit täydentää kunto-tietojasi niiden logiikan tietoja.

> [AZURE.NOTE] Watchdogs kuntoraportit näkyvät vain *sen jälkeen, kun* järjestelmäosien luominen kohteen. Kun kohde poistetaan, terveys-kaupasta poistaa kaikki liittyy kuntoraportit. Sama koskee kohteen uuden esiintymän luomisen yhteydessä (esimerkiksi uuden palvelun replikan esiintymän luodaan). Kaikkien raporttien vanha esiintymän liittyvät poistetaan ja kaupasta Siivotaan.

Järjestelmäosa raportoi tunnistetaan lähde, joka alkaa "**järjestelmän.**" etuliite. Watchdogs voi käyttää samaa etuliite niiden lähteistä, kuten virheelliset parametrit-raportit on hylätty.
Katsotaan joitakin järjestelmän raportteja ymmärtää, mitä käynnistää niitä ja miten voit korjata ne edustavat mahdolliset ongelmat.

> [AZURE.NOTE] Palvelun kangasta säilyy raporttien lisääminen halutut ehdot, jotka parantavat näkyvyys klusterin ja sovelluksen tapahtumat.

## <a name="cluster-system-health-reports"></a>Klusterin järjestelmän kuntoraportit
Klusterin kunto kohde luodaan automaattisesti kunto-kaupasta. Jos kaikki toimii oikein, se ei ole järjestelmän raportin.

### <a name="neighborhood-loss"></a>Ympäristö tappio
**System.Federation** ilmoittaa virheestä, kun se havaitsee ympäristö tappio. Raportti on yksittäisiä solmujen eikä Solmutunnus sisältyy ominaisuuden nimi. Jos yksi ympäristö ole käytettävissä koko palvelun kangasta soi, voit tavallisesti odottaa kaksi tapahtumaa (molemmille puolille välin raportin). Lisää asuintaloissa menetetään, jos sinun on tapahtumien.

Raportti näyttää Yleinen varauksen aikakatkaisu time to live. Raportin uudelleen jokaisen puolet TTL kesto, kunhan ehto pysyy aktiivisena. Tapahtuma poistetaan automaattisesti, kun se päättyy. Poista vanhentuneet toiminta varmistaa, että raportti on Siivotaan kunto-kaupasta oikein, vaikka raportoinnin solmu ei ole käytettävissä.

- **SourceId**: System.Federation
- **Ominaisuus**: **ympäristö** alkaa solmu tiedot
- **Seuraavat vaiheet**: Selvitä, miksi menetetään ympäristö (esimerkiksi Tarkista välisen klusterin).

## <a name="node-system-health-reports"></a>Solmun järjestelmän kuntoraportit
**System.FM**, joka edustaa automaattisesti hallintapalvelu on myöntäjä, joka hallitsee klusterin tietoja. Kukin solmu on yksi raportin System.FM, jossa tilaan. Solmu-kohteet poistetaan, kun solmu tila on poistettu (katso [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Solmun ylös/alas
System.FM raportit OK, kun solmu yhdistää soi (se on käytön). Se ilmoittaa virheestä, kun solmu poistuu soi (se on alaspäin joko päivittämiseen tai yksinkertaisesti koska se on epäonnistunut). Kunto-kaupan muodostama kunto hierarkian kestää toiminnon käyttöön korrelaatio System.FM solmu-raportteja sisältäviä kohteita. Katsomansa solmu virtual ylätason kaikkien käyttöön kohteiden. -Solmun käyttöön kohteiden tarjoamia kautta kyselyjen solmu ilmoittaa kuin jopa System.FM samaa esiintymää, kun esiintymän liittyville mukaan. Kun System.FM ilmoittaa, että solmu ei ole käytettävissä tai käynnistetään uudelleen (uuden esiintymän), terveys-kaupan tyhjentää automaattisesti käyttöön kohteita, jotka voivat olla vain alaspäin solmu tai Edellinen solmun esiintymä.

- **SourceId**: System.FM
- **Ominaisuus**: tila
- **Seuraavat vaiheet**: Jos solmu on alaspäin päivitys, se kannattaa tulee takaisin kun se on päivitetty. Tässä tapauksessa kuntotietojen voi vaihtaa takaisin OK. Jos solmu ei vastaanoteta takaisin tai epäonnistuu, ongelma on enemmän tutkimuksen.

Seuraavassa esimerkissä System.FM tapahtumaa, jonka kuntotietojen OK, solmun:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Vanhentumisen
**System.FabricNode** raportit varoituksen, kun solmun käyttämät varmenteet ovat lähellä vanhentumispäivä. On kolme solmu todistuksista: **Certificate_cluster**, **Certificate_server**ja **Certificate_default_client**. Vanhenemisen on vähintään kaksi viikkoa heti, kun raportti kuntotietojen on OK. Vanhenemisen ollessa kahden viikon ajalta raporttityyppi on varoitus. Näitä tapahtumia TTL on äärettömän, ja ne poistetaan, kun solmu jättää klusterin.

- **SourceId**: System.FabricNode
- **Ominaisuus**: **varmenteen** alkaa, ja se sisältää lisätietoja varmenteen tyyppi
- **Seuraavat vaiheet**: Päivitä varmenteet, jos ne ovat lähellä vanhentumispäivä.

### <a name="load-capacity-violation"></a>Lataa kapasiteetin rikkomuksesta
Palvelun kangasta kuormituksen raportit varoitus, jos se havaitsee solmu kapasiteetin vastoin.

 - **SourceId**: System.PLB
 - **Ominaisuus**: alkaa **kapasiteetti**
 - **Seuraavat vaiheet**: Tarkista annettu arvot ja näyttää nykyisen kapasiteetin solmun.

## <a name="application-system-health-reports"></a>Sovelluksen järjestelmän kuntoraportit
**System.CM**, joka edustaa klusterin hallintapalvelu on myöntäjä, joka hallitsee sovelluksen tietoja.

### <a name="state"></a>Tila
System.CM raportit kuin OK, kun sovellus on luotu tai päivitetty. Se ilmoittaa kunto-kaupan sovellus on poistettu, kun niin, että se voidaan poistaa-kaupasta.

- **SourceId**: System.CM
- **Ominaisuus**: tila
- **Seuraavat vaiheet**: Jos sovellus on luotu, se sisältää klusterin hallinnan kuntoraportti. Tarkista, lähettämällä kyselyn sovelluksen tila (esimerkiksi PowerShell cmdlet * *Get-ServiceFabricApplication ApplicationName *applicationName***).

Seuraavassa esimerkissä näkyy tilan tapahtuma **kangasta: / WordCount** sovelluksen:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Palvelun järjestelmän kuntoraportit
**System.FM**, joka edustaa automaattisesti hallintapalvelu on myöntäjä, joka hallitsee tietoja palveluista.

### <a name="state"></a>Tila
System.FM raportit kuin OK kun palvelu on luotu. Se poistaa kohteen kunto-kaupasta kun palvelu on poistettu.

- **SourceId**: System.FM
- **Ominaisuus**: tila

Seuraavassa esimerkissä esitetään tilan tapahtuma-palvelusta **kangasta: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Sijoittamattomat replikoiden rikkomuksesta
**System.PLB** raportit varoitus, jos se ei löydä yhden tai useamman palvelun replikoida sijainti. Raportin poistetaan, kun se päättyy.

- **SourceId**: System.FM
- **Ominaisuus**: tila
- **Seuraavat vaiheet**: Tarkista palvelun rajoitukset ja nykyisen tilan sijaintia.

Seuraavassa esimerkissä esitetään rikkomuksesta palvelun määritetty kohde 7 replikoiden klusterin 5 solmujen kanssa:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
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
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Osion järjestelmän kuntoraportit
**System.FM**, joka edustaa automaattisesti hallintapalvelu on myöntäjä, joka hallitsee osiot tietoja.

### <a name="state"></a>Tila
System.FM raportit kuin OK, kun osio on luotu ja on kunnossa. Se poistaa kohteen kunto-kaupasta osion poistamisen yhteydessä.

Jos osio on pienin replikan Laske alla, se ilmoittaa virheestä. Jos osio ei ole vähintään replikan Laske alla, mutta se on alle kohde replikan määrä, raportoi varoitus. Jos osio on quorum tappio, System.FM ilmoittaa virheestä.

Muita tärkeitä tapahtumia sisällyttää varoituksen, kun uudelleenmääritys kestää kauemmin odotettua ja luo kestää odotettua kauemmin. Odotettu kertaa muodosta ja kokoonpanon uudelleenmääritys on määritettävä perusteella palvelun skenaarioita. Esimerkiksi jos palvelu on teratavun valtion, kuten SQL-tietokantaan, Luo kestää kauemmin kuin palvelun tilan on vähän.

- **SourceId**: System.FM
- **Ominaisuus**: tila
- **Seuraavat vaiheet**: kuntotietojen tarkistus ei ole OK, jos se on mahdollista, että jotkin replikoita ei ole luotu, avata, tai ylemmän tason ensisijaisen tai toissijaisen oikein. Pääkansio syynä on monta tilanteissa käyttöönoton avoin tai muuta rooli-palvelun ohjelmavirhe.

Seuraavassa esimerkissä esitetään kunnossa osio:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Seuraavassa esimerkissä esitetään osio, joka on pienempi kuin kohde replikan Laske kunto. Seuraavaksi saat osion kuvaus, joka näyttää, miten se on määritetty: **MinReplicaSetSize** on kaksi ja **TargetReplicaSetSize** on seitsemän. Hae sitten klusterin solmujen määrän: viisi. Niin tässä tapauksessa replikat ei voi sijoittaa.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Replikan rajoituksen rikkomuksesta
**System.PLB** raportit varoitus, jos se havaitsee replikan rajoite-rikkomuksesta ja voi sijoittaa replikoiden osion.

- **SourceId**: System.PLB
- **Ominaisuus**: **ReplicaConstraintViolation** alkaa

## <a name="replica-system-health-reports"></a>Replikan järjestelmän kuntoraportit
**System.RA**, joka edustaa kokoonpanon uudelleenmääritys agent-komponentti on replikan osavaltion myöntäjä.

### <a name="state"></a>Tila
**System.RA** raportit kuin OK, kun se on luotu.

- **SourceId**: System.RA
- **Ominaisuus**: tila

Seuraavassa esimerkissä esitetään kunnossa replikan:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Replikan Avaa tila
Tämä kuntoraportti kuvaus sisältää alkamisajan (UTC-ajasta), kun API-kutsu on kutsuttu.

**System.RA** raportteja varoitus, jos se avaa kestää kauemmin kuin määritetty määräaika (oletusarvo: 30 minuuttia). Jos Ohjelmointirajapinnan vaikuttaa palvelun käytettävyys-raportti on myöntänyt paljon nopeammin (määritettävä aikaväli, ja oletusarvo on 30 sekuntia). Mitattu aika sisältää Monistus-avaaminen ja palvelun Avaa aika. Ominaisuuden muuttuu OK, jos Avaa on valmis.

- **SourceId**: System.RA
- **Ominaisuus**: **ReplicaOpenStatus**
- **Seuraavat vaiheet**: Jos kuntotietojen tarkistus ei ole OK, tutkia, miksi se avaa kestää odotettua kauemmin.

### <a name="slow-service-api-call"></a>Hidas service API-kutsu
**System.RAP** ja **System.Replicator** raportin varoitus, jos käyttäjä palvelukoodi puhelun kestää kauemmin kuin määritetyn ajan. Varoitus ole valittu, kun kutsu on valmis.

- **SourceId**: System.RAP tai System.Replicator
- **Ominaisuus**: hidas Ohjelmointirajapinnan nimi. Kuvaus on lisätietoja Ohjelmointirajapinnan on odotetaan aika.
- **Seuraavat vaiheet**: Selvitä, miksi puhelun kestää odotettua kauemmin.

Seuraavassa esimerkissä osion quorum tappio ja selvittää, miksi valmis tutkimuksen ohjeiden mukaisesti. Jokin replikoita on varoitus kuntotietojen, niin saat sen kunto. Se näyttää, että palvelutoiminto kestää odotettua kauemmin, tapahtuman System.RAP mukaan. Kun tiedot on vastaanotettu, seuraava vaihe on siellä tutkia ja tarkista palvelun-koodin. Tässä tapauksessa tilallisten palvelun **RunAsync** käyttöönoton ilmoittaa käsittelemättömän poikkeuksen vuoksi. Replikat ovat kierrättäminen, joten et ehkä näe minkä tahansa replikoiden varoitus-tilassa. Voit yrittää käytön kuntotietojen ja Etsi eroja replikan ID-tunnuksellasi. Tietyissä tapauksissa uudelleenyritykset voi antaa vihjeitä.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Valitse virheenkorjaus viallinen sovelluksen käynnistyessä diagnostiikan tapahtumat windows Näytä RunAsync-poikkeus:

![Visual Studio 2015 diagnostiikan tapahtumat: kangasta RunAsync virhe: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnostiikan tapahtumat: Virhe RunAsync **kangasta: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Replikoinnin jonon koko
**System.Replicator** raportteja varoitus, jos replikoinnin jono on täynnä. Valitse Perus tämä tapahtuu yleensä koska yksi tai useampi toissijainen replikoida on hidasta Kuittaa toimintoja. Valitse Toissijainen tämä tapahtuu yleensä kun palvelu on hidas, voit käyttää toimintoja. Varoitus ole valittu, kun jonossa ei ole enää koko.

- **SourceId**: System.Replicator
- **Ominaisuus**: **PrimaryReplicationQueueStatus** tai **SecondaryReplicationQueueStatus**, sen mukaan replikarooli

### <a name="slow-naming-operations"></a>Nimeäminen hidas toiminnot

**System.NamingService** raportit kunto sen ensisijainen replikan kun Naming toiminto kestää kauemmin kuin hyväksyttävä. Esimerkkejä Naming toiminnot ovat [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) tai [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Lisätietoja tavoista löytyy FabricClient, valitse esimerkiksi kohdassa [service management menetelmät](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) ja [ominaisuuden hallinnan kautta](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] Naming palvelun selvittää palveluiden nimet klusterin sijaintiin, ja käyttäjät voivat hallita palveluiden nimet ja ominaisuudet. Palvelun kangasta osioitu samanlainen palvelu on. Yksi osiot edustaa myöntäjä omistaja, jossa on metatietoa kaikki palvelun kangasta nimet ja palveluja. Palvelun kangasta nimet yhdistetään eri osiot, kutsutaan omistajan nimi osiot niin, että palvelu on laajennettava. Lisätietoja [Naming palvelu](service-fabric-architecture.md).

Kun Naming toiminto kestää odotettua kauemmin, toiminto on Vääränlainen varoitus raportin *ensisijainen replikan Naming service-osion toiminto toimii*. Jos toiminto on valmis, Varoitus ole valittu. Jos toiminto on valmis ja järjestelmä antaa virheilmoituksen, kuntoraportti sisältää virheistä.

- **SourceId**: System.NamingService
- **Ominaisuus**: alkaa **Duration_** etu- ja määrittää hidas toiminta ja palvelun kangasta nimi, jossa toimintoa käytetään. Esimerkiksi jos palvelun luonti nimi kangasta: / MyApp/MyService kestää liian kauan, ominaisuus on Duration_AOCreateService.fabric:/MyApp/MyService. Lentoliikenteen Harjoittajien asioista roolin Naming osion nimeä ja toiminto.
- **Seuraavat vaiheet**: Miksi Naming epäonnistuu valintaruutu. Kunkin toiminnon voi olla eri syiden. Poista esimerkiksi palvelun voi olla jumissa solmun, koska sovelluksen isäntä säilyttää kaatuu solmun vuoksi käyttäjä ohjelmavirhe service-koodin.

Seuraavassa esimerkissä esitetään palvelun luontitoiminto. Toiminnon kulunut yli määritetty. Lentoliikenteen Harjoittajien yrittää suorittaa uudelleen ja lähettää työn ei. EI edellisen toiminnon aikakatkaisu kanssa. Tässä tapauksessa saman se on ensisijainen lentoliikenteen Harjoittajan ja ei ole roolit.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication järjestelmän kuntoraportit
**System.Hosting** on otettu käyttöön kohteiden myöntäjä.

### <a name="activation"></a>Aktivointi
System.Hosting raportit kuin OK, kun sovellus on otettu käyttöön solmu. Muussa tapauksessa ilmoittaa virheestä.

- **SourceId**: System.Hosting
- **Ominaisuus**: aktivointi, mukaan lukien rahoittaja-versio
- **Seuraavat vaiheet**: Jos sovellus on perusasemassa, tutkia, miksi aktivointi epäonnistui.

Seuraavassa esimerkissä esitetään onnistuneen aktivoinnin:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Lataa
**System.Hosting** ilmoittaa virheestä, jos sovellus paketin lataaminen epäonnistuu.

- **SourceId**: System.Hosting
- **Ominaisuus**: * *Lataa:*RolloutVersion***
- **Seuraavat vaiheet**: Selvitä, miksi solmu lataaminen epäonnistui.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage järjestelmän kuntoraportit
**System.Hosting** on otettu käyttöön kohteiden myöntäjä.

### <a name="service-package-activation"></a>Palvelun paketin aktivointi
System.Hosting raportit niin OK palvelun paketin aktivointi solmun onnistuu. Muussa tapauksessa ilmoittaa virheestä.

- **SourceId**: System.Hosting
- **Ominaisuus**: aktivointi
- **Seuraavat vaiheet**: Selvitä, miksi aktivointi epäonnistui.

### <a name="code-package-activation"></a>Koodin paketin aktivointi
**System.Hosting** raportit kuin OK kunkin koodi-paketin aktivointi onnistuu. Jos aktivointi epäonnistuu, raportoi varoitus mukaisena. Jos **CodePackage** aktivointi ei onnistu, tai lopettaa virheen suurempi kuin määritetty **CodePackageHealthErrorThreshold**, jossa ilmoittaa virheestä. Jos palvelun paketti sisältää useita koodi-paketteja, aktivointi-raportti on luotu kullekin.

- **SourceId**: System.Hosting
- **Ominaisuus**: etuliite **CodePackageActivation** ja sisältää nimen ja koodi-paketin aloituskohtaa kuin * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/aloituskohta* ** (esimerkiksi **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Palvelun tyyppi rekisteröinti
**System.Hosting** raportit niin OK, jos palvelutyypin on rekisteröity onnistuneesti. Se ilmoittaa virheestä, jos rekisteröinti ei ole tehnyt aika (sellaisena kuin se on määritetty käyttämällä **ServiceTypeRegistrationTimeout**). Jos palvelutyypin ei ole rekisteröity solmun, tämä johtuu siitä suorituksen aikana on suljettu. Varoitus isännöinnin raportit.

- **SourceId**: System.Hosting
- **Ominaisuus**: etuliite **ServiceTypeRegistration** ja sisältää palvelunimi (esimerkiksi **ServiceTypeRegistration:FileStoreServiceType**)

Seuraavassa esimerkissä esitetään kunnossa käyttöön palvelupakettiin:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Lataa
**System.Hosting** ilmoittaa virheestä, jos palvelun paketin lataaminen epäonnistuu.

- **SourceId**: System.Hosting
- **Ominaisuus**: * *Lataa:*RolloutVersion***
- **Seuraavat vaiheet**: Selvitä, miksi solmu lataaminen epäonnistui.

### <a name="upgrade-validation"></a>Päivitä vahvistus
**System.Hosting** ilmoittaa virheestä, jos päivityksen aikana vahvistus epäonnistuu, tai jos päivitys epäonnistuu solmun.

- **SourceId**: System.Hosting
- **Ominaisuus**: etuliite **FabricUpgradeValidation** ja sisältää päivityksen
- **Kuvaus**: osoittaa virheen

## <a name="next-steps"></a>Seuraavat vaiheet
[Palvelun kangasta kuntoraporttien tarkasteleminen](service-fabric-view-entities-aggregated-health.md)

[Raportin ja tarkista palvelun kunto](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Valvo ja vianmäärityksen paikallisesti palvelut](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Palvelun kangasta sovelluksen päivitys](service-fabric-application-upgrade.md)
