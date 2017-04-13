<properties
   pageTitle="Palvelun kangasta klusterin resurssien hallinta - asettelun käytännöt | Microsoft Azure"
   description="Lisää sijainti käytännöt ja säännöt kangasta palveluille yleiskatsaus"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Asettelun käytännöt kangasta palveluille
On useita eri lisäsääntöjä, jotka voivat sinut caring tietoja, jos palvelun kangasta klusterin on koottu yli maantieteelliset etäisyydet sano useita palvelinkeskusten tai Azure alueet, tai jos ympäristösi ulottuu useisiin alueisiin geopoliittisten ohjausobjektin (tai joissakin muissa tapauksissa, jossa sinulla oikeudelliset ja käytännöistä rajat tärkeisiin tai yhdistävää etäisyydet todellinen suorituskykyä ja viive vaikuttaa). Useimmat näistä voitu määrittää solmun ominaisuudet ja sijoittaminen rajoitukset kautta, mutta osa on monimutkaisempi. Voit tehdä asioita yksinkertaisempi annamme nämä muita commmands. Aivan kuten ajoitukseen muita rajoituksia sijaintia käytännöt voidaan määrittää kohti nimeltä palvelun esiintymän välein.

## <a name="specifying-invalid-domains"></a>Virheellinen toimialueiden määrittäminen
InvalidDomain sijainti-käytännön avulla voit määrittää, että tietyn vika toimialue on virheellinen tämän kuormituksen. Tämän käytännön varmistaa, että tietyn palvelun suorittaa koskaan tietyn alueen, kuten geopoliittisten tai yrityksen käytännön syistä. Virheellinen useita toimialueita voidaan määrittää erilliset käytännöt kautta.

![Virheellinen toimialue-Esimerkki][Image1]

Koodi:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShellin:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Pakollinen toimialueiden määrittäminen
Toimialue sijaintia käytäntö edellyttää, että kaikki tilallisten replikoiden tai tilattomien palveluesiintymiä palvelun sijaittava määritettyä toimialuetta. Useita tarvittavat toimialueita voidaan määrittää erilliset käytännöt kautta.

![Toimialue-Esimerkki][Image2]

Koodi:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShellin:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Ensisijainen toimialue ensisijainen replikoiden määrittäminen
Ensisijainen ensisijainen toimialue on mielenkiintoista komponentti, koska se sallii vika toimialue, jossa ensisijaisen sijoitetaan, jos se on mahdollista tehdä valinnan. Kun kaikki on kunnossa ensisijainen päätyvät tämän toimialueen. Toimialueen tai ensisijainen replikan epäonnistuvat tai sulkea jostain syystä ensisijainen siirretään toiseen sijaintiin. Jos tämä sijainti ei ole ensisijainen toimialue, valitse mahdollista klusterin Resurssienhallinta siirtää sen takaisin ensisijainen toimialue. Tämä asetus on luonnollisesti vain järkevää tilallisten Services. Käytäntö on eniten hyötyä klustereissa, johon on koottu Azure alueiden tai useita palvelinkeskusten yli. Näissä tilanteissa käyttämäsi redundancy kaikista sijainneista, mutta mieluummin, että ensisijainen replikoiden sijoitetaan tietyssä paikassa, jos haluat säätää näytön viive toimille, joka on ensisijainen Siirry (kirjoittaa ja oletusarvoisesti kaikki lukee ovat toimii myös ensisijainen).

![Ensisijainen ensisijainen toimialueet ja automaattisesti][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShellin:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Edellyttävän replikat voi jakaa kaikki toimialueet ja estävä pakkaus
Voit määrittää toiseen käytäntö määritetään edellyttämään replikat aina voi jakaa käytettävissä vika toimialueet. Tämä tapahtuu oletusarvoisesti useimmissa tapauksissa, jossa klusterin on kunnossa kuitenkin on degenerate tapauksia, jossa replikoiden varten määritettyä osiota saattaa sinut tilapäisesti pakattu yksittäisen vika tai Päivitä toimialueen. Esimerkiksi oletetaan, vaikka klusterin on 9 solmujen 3 vika Domains (0, 1 ja 2) ja palvelussa on 3 replikoiden, solmut, joka on käytössä replikat vika toimialueiden 1 ja 2 tapahtui ja kapasiteetin ongelmien vuoksi mikään näistä toimialueista muiden solmujen on voimassa. Jos palvelun kangasta luonnissa replikat korvaavan, klusterin Resurssienhallinta on tallennettava vika toimialueen 0, mutta, joka luo tilanteeseen, jossa vika toimialueen rajoituksen rikkoo. Se myös kasvaa mahdollisuutta, että koko replikajoukon voi hävitä (Jos Suodatuspalvelu 0 on voidaan menettää permananently). (Lisätietoja rajoitteiden ja rajoite prioriteetit yleensä Tutustu [tämän ohjeaiheen](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Jos nähnyt koskaan kunto varoitus, kuten "kuormituksen havaitsi rajoite-rikkomuksesta varten tämän replikan: kangasta: /<some service name> toissijainen osion <some partition ID> on rikkomiseen rajoituksen: FaultDomain" olet saavuttanut ongelmaa tai suunnilleen se. Näissä tilanteissa eivät yleensä lyhytkestoisia (solmut ei pysy alaspäin pitkään tai jos kuin ja annettava luominen korvauksia siellä on solmujen oikea vika toimialueet on kelvollinen), mutta jotkin toiminnoista, jotka mieluummin kaupan niiden replikoiden tietojen menettämisen riskiä käytettävyys. Emme voi tehdä määrittämällä "RequireDomainDistribution"-käytäntö, joka takaa, että ei replikat samasta osiosta sallitaan koskaan saman vika tai Päivitä toimialueen.

Jotkin työmääriä mieluummin käyttää kohde-määrä (kopiot-tila) on lainkaan (vedonlyönti vastaan yhteensä toimialueen virheet ja tietää, että ne tavallisesti palauttaa paikallinen tila) aikojen muiden mieluummin otettava käyttökatkot vanhempia kuin riskin oikeellisuuden ja dataloss koskee. Koska useimmat tuotannon työmääriä suorittaa yli 3 replikoiden kanssa, oletusarvo on ei edellytä toimialueen jakauman ja anna tasaamisen ja käsitellä tapauksissa normaalisti, vaikka, joka tarkoittaa, että tilapäisesti toimialue on pakattu siihen useita replikoita automaattisesti.

Koodi:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShellin:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nyt se on mahdollista määritysten käyttäminen palvelujen klusterin, joka on ei maantieteellisesti koottu? Varmista, että voit! Mutta ei ole hyvä syy liian – etenkin tarvittavat, virheellinen ja ensisijainen toimialue-määrityksiä voidaan välttää, paitsi, jos käytössäsi on todella maantieteellisesti koottuja klusterin – ei kannata minkä tahansa yrittää pakottaa annetun työmäärää, voit suorittaa yhteen Teline tai mieluummin joitakin paikallisen klusterin segmentin toisen yläpuolelle, ellei erityyppisiä laitteiston tai työmäärää Segmentointi ajan tasalla , ja tapauksissa voi ratkaista Normaali sijaintia rajoitukset kautta.

## <a name="next-steps"></a>Seuraavat vaiheet
- Jos haluat lisätietoja muiden määrittämiseen käytettävissä olevat asetukset services uloskuittaus klusterin Resurssienhallinta-määrityksiä aiheen käytettävissä [tietoja palveluiden määrittäminen](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
