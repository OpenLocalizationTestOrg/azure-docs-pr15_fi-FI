<properties
   pageTitle="Vianmääritys sovelluksen päivitykset | Microsoft Azure"
   description="Tässä artikkelissa käsitellään joitakin yleisiä ongelmia ympärille palvelun kangasta-sovellus ja kuinka voit ratkaista ne."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="troubleshoot-application-upgrades"></a>Vianmääritys sovelluksen päivitykset

Tässä artikkelissa käsitellään joitakin yleisiä ongelmia ympärille sovelluksen Azure palvelun kangasta ja kuinka voit ratkaista ne.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Vianmääritys epäonnistui sovelluksen päivitys

Kun päivitys epäonnistuu, **Hae ServiceFabricApplicationUpgrade** -komento on lisätietoja virheenkorjaus virheen.  Seuraavassa luettelossa määrittää, miten lisätiedot käytetään:

1. Määritä virheen tyyppi.
2. Määritä virheen syy.
3. Paikantaa Lisää vähintään yksi epäonnistumista osat.

Nämä tiedot ovat käytettävissä, kun palvelua kangasta havaitsee virheen riippumatta siitä, onko **FailureAction** aikaisempi tai keskeyttää päivityksen.

### <a name="identify-the-failure-type"></a>Määritä virheen tyyppi

**Hae ServiceFabricApplicationUpgrade**tulostus **FailureTimestampUtc** tunnistaa aikaleima (UTC-ajan), jolla päivitysvirhe havaitsi palvelun kangasta ja **FailureAction** on aiheutunut. **FailureReason** yksilöi jonkin kolme mahdollisen virheen korkean tason syitä:

1. UpgradeDomainTimeout - ilmaisee, että tietyn päivityksen toimialueen noudatit liian kauan ja **UpgradeDomainTimeout** on vanhentunut.
2. OverallUpgradeTimeout - ilmaisee, että yleinen päivitys noudatit liian kauan ja **UpgradeTimeout** on vanhentunut.
3. HealthCheck - ilmaisee, että päivitys toimialueen päivityksen jälkeen sovellus olleet perusasemassa määritetyn kunto käytäntöjen mukaan ja **HealthCheckRetryTimeout** on vanhentunut.

Nämä tietueet vain näkyvät tulosteen kun päivitys epäonnistuu ja käynnistää peruutetaan. Lisätietoja näkyy virheen tyypin mukaan.

### <a name="investigate-upgrade-timeouts"></a>Tutkia päivityksen aikakatkaisu

Päivitä virheet johtuvat yleisimmin palvelun käytettävyysongelmia aikakatkaisu. Tämän kappaleen jälkeen tulos on tyypillinen päivitykset missä palvelun replikoiden tai esiintymät eivät käynnisty uusi koodi-versiossa. **UpgradeDomainProgressAtFailure** kentän sieppaa virheen milloin tahansa odotetaan päivityksen työn tilannevedoksen.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

Tässä esimerkissä päivitys epäonnistui päivityksen toimialueen *MYUD1* ja kaksi osiota (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* ja *4b43f4d8-b26b-424e-9307-7a7a62e79750*) on jumissa. Osiot on jumissa, koska suorituksen ei voinut sijoittaa kohde solmujen *Node1* ja *Node4*ensisijainen replikoiden (*WaitForPrimaryPlacement*).

**Hae ServiceFabricNode** -komennon avulla voidaan varmistaa, että nämä kaksi solmut ovat päivityksen toimialueen *MYUD1*. *UpgradePhase* lukee *PostUpgradeSafetyCheck*, mikä tarkoittaa, että turvallisuutta tarkistukset ovat jälkeinen kaikki solmut päivityksen toimialueen tehnyt päivittäminen. Nämä tiedot osoittaa mahdolliset ongelma koodi sovelluksen uuden version. Yleisimmät ongelmat ovat virheiden Avaa tai muuntaa ensisijainen koodin polkuja.

*UpgradePhase* *PreUpgradeSafetyCheck* , tarkoittaa, että käytettävissä on ongelmia, Päivitä toimialueen valmistelu, ennen kuin se suoritettiin. Yleisimmät kysymykset ovat tällöin virheiden Sulje tai ensisijainen koodin poluista alennus.

Nykyinen **UpgradeState** on *RollingBackCompleted*, joten alkuperäinen päivitys suoritettu kanssa peruuttaminen **FailureAction**, jossa automaattisesti peruutetaan päivitys virheen yhteydessä. Jos alkuperäinen päivitys on tehty manuaalinen **FailureAction**kanssa, valitse päivitys sen sijaan voidaan keskeytystilassa sallimaan live virheenkorjaus sovelluksen.

### <a name="investigate-health-check-failures"></a>Tutkia kuntotietojen tarkistus-virheet

Kuntotietojen tarkistus-virheet voivat käynnistämä eri ongelmat, jotka voi esiintyä päätyttyä kaikki solmut päivityksen toimialueen päivittäminen ja kulkeva turvallisuutta kaikki tarkistukset. Tämän kappaleen jälkeen tulos on tyypillinen päivitysvirhe vuoksi epäonnistui kuntotietojen tarkistus. **UnhealthyEvaluations** kentän sieppaa tilannevedoksen kuntotietojen tarkistukset, jotka yhteydessä määritettyä [kuntokäytännön](service-fabric-health-introduction.md)mukaan päivitys epäonnistui.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Kuntotietojen tarkistus virheiden tutkiminen vaatii ensin hallitsemista kangasta palvelun kunto-mallin. Mutta tällaisen tarkempia tietoja, myös ilman näkyvissä kaksi palvelut ovat perusasemassa: *kangasta: / DemoApp/Svc3* ja *kangasta: / DemoApp/Svc2*, sekä virhe kuntoraportit ("InjectedFault" tässä tapauksessa). Tässä esimerkissä kaksi poissa neljään-palvelut ovat perusasemassa, joka on 0 % perusasemassa (*MaxPercentUnhealthyServices*) oletusarvon kohde alla.

Päivitys on keskeytetty yhteydessä epäonnistumista määrittämällä **FailureAction** manuaalisen päivityksen käynnistyksen yhteydessä. Tässä tilassa pystyy tutkia live-järjestelmässä tilaan ennen siihen enempää toiminto.

### <a name="recover-from-a-suspended-upgrade"></a>Keskeytetty päivityksen palauttaminen

Peruutus **FailureAction**, jossa on ei tarvita, sillä päivityksen automaattisesti peruutetaan yhteydessä epäonnistumista palauttaminen. Manuaalinen **FailureAction**, jossa on useita palautusasetukset:

1. Käynnistää manuaalisesti peruuttaminen
2. Käy läpi loput päivityksen manuaalisesti
3. Jatka valvottu päivitys

**Käynnistä ServiceFabricApplicationRollback** -komennon avulla voidaan milloin tahansa Käynnistä sovellus peruutetaan. Kun komento palauttaa onnistuneesti, peruutus-pyyntö on rekisteröity järjestelmässä ja aloittaa pian sen jälkeen.

**Jatka ServiceFabricApplicationUpgrade** -komennon avulla voidaan käydään läpi loput päivityksen manuaalisesti, Päivitä toimialueen kerrallaan. Tässä tilassa vain turvallisuutta tarkistukset suoritetaan järjestelmä. Ei enää kuntotietojen tarkistus suoritetaan. Tämä komento voi käyttää vain, kun *UpgradeState* näkyy *RollingForwardPending*, mikä tarkoittaa, että nykyinen päivityksen toimialue on valmis päivittäminen mutta seuraavaan ei ole aloitettu (odottaa).

**Päivityksen ServiceFabricApplicationUpgrade** -komentoa voidaan käyttää, kun haluat jatkaa valvottu päivityksen kanssa sekä turvallisuus ja kuntotietojen tarkistukset tehdä.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

Päivityksen säilyy päivityksen toimialueen, johon se on viimeksi keskeytetty ja sama päivittäminen parametrit- ja kuntotietojen käytännöt kuin ennen käyttöä. Tarvittaessa seuraavia päivityksen parametrit ja terveyteen käytännöt Edellinen tulos näkyy voidaan muuttaa sama komento kun päivitys jatkaa. Tässä esimerkissä päivitys on jatkaa Tarkkailtavat tilassa parametrit-ja kuntotietojen käytännöt ei muutu.

## <a name="further-troubleshooting"></a>Lisää vianmääritysohjeita

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Palvelun kangasta ei seuraa määritetyn kuntokäytännöt

Mahdollinen syy 1:

Palvelun kääntää kaikki prosenttien todellisia lukuja kunto arvioitavaksi kohteiden (esimerkiksi replikoiden, osiot ja palvelut) ja pyöristää aina ylöspäin koko kohteisiin. Esimerkiksi jos on viisi replikoiden suurin _MaxPercentUnhealthyReplicasPerPartition_ on 21 prosenttia, valitse palvelu kangasta sallii enintään replikat perusasemassa (kyseisen is,'Math.Ceiling (5\*0.21)). Näin ollen kuntokäytännöt kannattaa asettaa vastaavasti.

Mahdollinen syy 2:

Kuntotietojen käytännöt määritetään kannalta yhteensä services ja eivät tiettyjä palvelun esiintymät. Esimerkiksi ennen päivitystä, jos sovellus on neljä palvelun esiintymät A, B, C ja D-palvelun D on perusasemassa mutta vaikutusta sovellukseen. Haluat ohittaa tunnetut perusasemassa palvelun D päivitys ja määritä parametri on 25 %, jos vain A, B ja C *MaxPercentUnhealthyServices* on oltava kunnossa aikana.

Kuitenkin päivityksen aikana D saattaa tulla kunnossa samalla, kun C tulee perusasemassa. Päivityksen edelleen toiminto onnistuu vain 25 % palvelut ovat perusasemassa. Se saattaa johtaa odottamattomiin virheiden vuoksi C parhaillaan odottamatta perusasemassa sijaan D. Tässä tilanteessa D olisi voidaan mallintaa eri palvelutyyppi a, B ja c-näppäinyhdistelmää. Koska kuntokäytännöt on määritetty kohti palvelutyyppi, eri perusasemassa prosentti raja-arvot voidaan lisätä eri palveluissa. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Sovelluksen päivitys kunto käytäntöä ei ole määrittänyt, mutta päivitys epäonnistuu joissakin aikakatkaisu, joka ei koskaan määritettyä varten

Kun kuntokäytännöt eivät ole annettu päivityksen pyynnön, haetaan *ApplicationManifest.xml* sovelluksen nykyisen version. Esimerkiksi jos päivität sovelluksen X versio 1.0 parametrissa versiosta 2.0-sovelluksen kuntokäytännöt 1.0 version käytetään. Jos päivitys voidaan käyttää eri kunto-käytäntö, käytäntö on määritettävä sovelluksen päivityksen API-kutsu osana. Määritettyä API-kutsu käytäntöjä voi käyttää vain päivityksen aikana. Kun päivitys on valmis, käytetään määritetyn *ApplicationManifest.xml* käytännöt.

### <a name="incorrect-time-outs-are-specified"></a>Virheellinen aikakatkaisu on määritetty

Sinun on miettinyt siitä, mitä tapahtuu, kun aikakatkaisu määritetään yhtenäistä. Esimerkiksi voi olla *UpgradeTimeout* , joka on pienempi kuin *UpgradeDomainTimeout*. Vastaus on virhe palautetaan. Jos *UpgradeDomainTimeout* on pienempi kuin *HealthCheckWaitDuration* ja *HealthCheckRetryTimeout*summa tai *UpgradeDomainTimeout* on pienempi kuin *HealthCheckWaitDuration* ja *HealthCheckStableDuration*palauttaa virheitä.

### <a name="my-upgrades-are-taking-too-long"></a>Oma päivitykset kestää liian kauan

Viimeistele päivityksen ajan määräytyy aikakatkaisu, jotka on määritetty ja kuntotietojen tarkistus. Kuntotietojen tarkistus ja niiden aikakatkaisu määräytyvät sen mukaan, kuinka kauan rekisteröijän kopioiminen ja ottaa käyttöön tasapainota sovellus. On liian kohdetoimialueen aikakatkaisu kanssa voi tarkoittaa Lisää epäonnistui päivityksiä, jotta Suosittelemme alkaen käytetään pidempi aikakatkaisu.

Seuraavassa on lyhyt kertaus miten aikakatkaisu käsitellä päivityksen kertaa:

Päivittää, Päivitä toimialueen ei voi suorittaa nopeammin *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Päivitysvirhe et voi ilmetä nopeammin *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Päivitä toimialueen päivityksen ajan rajoittavat *UpgradeDomainTimeout*.  Jos sovellus kunto säilyttää siirtymistä edestakaisin *HealthCheckRetryTimeout* ja *HealthCheckStableDuration* ovat molemmat muuta kuin nolla, valitse päivitys myöhemmin aikakatkaistaan *UpgradeDomainTimeout*käyttöön. *UpgradeDomainTimeout* käynnistyy laskien, kun päivitys nykyisen päivityksen toimialueen alkaa.

## <a name="next-steps"></a>Seuraavat vaiheet

[Päivittäminen oman sovellus käyttää Visual Studio](service-fabric-application-upgrade-tutorial.md) esitellään sovelluksen päivityksen Visual Studiossa.

[Päivittäminen sovelluksen käyttämällä Powershell](service-fabric-application-upgrade-tutorial-powershell.md) esitellään sovelluksen päivityksen PowerShellin avulla.

Määrittää, miten sovelluksesi päivittää [Päivittäminen](service-fabric-application-upgrade-parameters.md)parametreilla.

Varmista sovelluksen päivitykset yhteensopivat opettelemalla käyttämään [Tietoja Sarjatoiminto](service-fabric-application-upgrade-data-serialization.md).

Opettele käyttämään kehittyneitä toimintoja siirtyessäsi sovelluksen viittaamalla [Lisäohjeita](service-fabric-application-upgrade-advanced.md).

Sovelluksen päivitykset yleisten ongelmien korjaaminen viittaamalla [Vianmääritys sovelluksen päivitykset](service-fabric-application-upgrade-troubleshooting.md)ohjeita.
 
