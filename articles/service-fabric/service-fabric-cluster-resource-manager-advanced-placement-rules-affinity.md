<properties
   pageTitle="Palvelun kangasta klusterin resurssien hallinta - affiniteetti | Microsoft Azure"
   description="Yleistä affiniteetti kangasta palveluille määrittäminen"
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

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Määrittäminen ja käyttäminen palvelun affiniteetti palvelun kangasta

Affiniteetti on ohjausobjekti, joka on annettu pääasiassa avulla suurempi Monoliittiset sovellusten siirtymisen helpottamiseksi cloud ja microservices maailman kyselyjä. Toisaalta se voi käyttää myös joissakin tapauksissa oikeutetut optimointi palvelujen suorituskyvyn parantaminen vaikka tämä voi olla vaikutukset.

Oletetaan, että olet joka tuo suurempi sovelluksen tai aiemmin vain ei suunniteltu microservices mielessä, palvelun kangasta. Siirtymän tämä on todella yleisiä ja Meillä oli useita asiakkaita (sisäisten ja ulkoisten) tässä tilanteessa. Voit aloittaa nosto koko Appin ympäristössä, se on pakattu käytön ja suorittaminen. Valitse aloitat purkaa eri pienempi services, että kaikki puhua toisiinsa.

Valitse on "Oops...". "Oops" kuuluu yleensä johonkin näistä luokista:

1. Jotkin osan X Monoliittiset sovelluksessa oli dokumentoimattomia riippuvuuden osan Y ja on käytössä vain ne yhdeksi erillisiä palveluita. Nämä ovat nyt käynnissä klusterin eri solmuissa, koska ne ovat katkennut.
2.  Seuraavat asiat ottaa heihin yhteyttä (paikallinen nimetyt putket | jaetun muistin | levylle), mutta haluat, että voit päivittää sen itsenäisesti vähän sujuu todella. Kiinteä riippuvuus poistaa myöhemmin.
3.  Kaikki on kunnossa, mutta se muuttuu nämä kaksi osat ovat itse asiassa hyvin chatty/suorituskyvyn luottamuksellisia. Kun ne siirtää niitä eri services yleistä sovelluksen suorituskyvyn tanked- tai viive kasvaa. Tämän vuoksi sovelluksen yleistä on täyttämättömien odotuksia.

Tällaisissa tapauksissa on halua menettää refactoring työn, etkä halua palaa monoliitin, mutta annettava joitakin paikka mielessä. Tämä on käytössä, joko Microsoft muuttamiseen osat toimivat luonnollisesti services, kunnes tai olemme ratkaista suorituskyvyn odotuksia jollakin muulla tavalla, jos mahdollista.

Mitä voi tehdä? Voit myös yrittää affiniteetti ottaminen käyttöön.

## <a name="how-to-configure-affinity"></a>Affiniteetti määrittäminen
Määrittämään affiniteetti määrittää kahden eri palvelun affiniteetti-suhteen. Voit ajatella affiniteetti "osoittamalla" yhden palvelun toiseen ja ajattelevat "Tämä palvelu voidaan suorittaa vain missä että palvelu on käynnissä." Joskus viitataan affiniteetti pää-/ alisisältötyypin suhde (Jos osoitat lapsen ylemmän tason). Affiniteetti varmistaa, että replikoiden tai yhden palvelun esiintymät sijoitetaan replikoiden tai esiintymät toiseen saman solmuissa.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Eri affiniteetti asetukset
Affiniteetti esitetään kautta jokin useita korrelaatio-järjestelmiä ja on kaksi eri tilaa. Affiniteetti yleisimmät tila on NonAlignedAffinity. NonAlignedAffinity replikoiden tai esiintymiä eri palveluissa sijoitetaan saman solmuissa. Muut tila on AlignedAffinity. Tasatut affiniteetti on hyödyllinen vain tilallisten-palvelujen kanssa. Kaksi tilallisten palveluiden on tasattu affiniteetti määrittäminen varmistaa näistä palveluista primaareista sijoitetaan saman solmuissa kuin toisiinsa. Se myös aiheuttaa kunkin kahdet näistä palveluista sijoittaa saman solmuissa secondaries. On myös mahdollista (mutta tavallista harvinaisempi) NonAlignedAffinity määrittämiseksi tilallisten palvelut. NonAlignedAffinity, kaksi tilallisten palvelujen eri replikassa collocated saman solmuissa, mutta ei yritys tehdään Tasaa niiden primaareista tai secondaries.

![Affiniteetti tila ja niiden vaikutukset][Image1]

### <a name="best-effort-desired-state"></a>Paras työmäärään haluttu tila
Affiniteetti ja Monoliittiset arkkitehtuureihin välillä joitakin eroja. Monia ne ovat, koska affiniteetti suhde on paras. Affiniteetti yhteydessä palvelut ovat olennaisesti erilaisia kohteista, joita voi epäonnistua ja siirretään erikseen. Saatavilla on myös syitä, miksi affiniteetti suhde voi katkaista. Esimerkiksi kapasiteettirajoitukset, jos vain tietyt palvelun objektien affiniteetti yhteyteen mahtuu annetun solmun. Tällaisissa tapauksissa affiniteetti suhde on määritetty, mutta sitä ei voi toteuttaa muiden rajoitusten vuoksi. Jos mahdollista, jos haluat säilyttää kaikki muut rajoitukset ja affiniteetti voi tehdä myöhemmin affiniteetti rajoituksen rikkomuksesta korjataan automaattisesti.  

### <a name="chains-vs-stars"></a>Ketjut ja tähteä
Tänään emme voi ottaa mallin ketjut affiniteetti yhteyksiä. Tämä tarkoittaa sitä, palvelu, joka on yksi affiniteetti yhteydessä ei voi olla ylätason toiseen affiniteetti yhteydessä. Jos haluat mallin tämäntyyppisen suhteen, sinun on tehokas malli täyttökuviona yhdistettyjen sijaan. Jotta voit tehdä tämän alimmat lapsen voidaan yläkäsite "Keskimmäinen" lapsen ylätason sijaan. Palvelujen järjestelyssä mukaan tämä saattaa edellyttää useita lasten ylätason yhteyshenkilönä "paikkamerkin" palvelun luominen.

![Ketjut ja tähteä affiniteetti yhteydet yhteydessä][Image2]

Huomaa affiniteetti yhteyksistä tänään on, että ne ovat suunta. Tämä tarkoittaa, että "affiniteetti" säännön pakottaa vain lapsen on ylemmän tason missä. Jos esimerkiksi ylätason yhtäkkiä epäonnistuu päälle toisen solmun sitten klusterin Resurssienhallinta ei varsinaisesti Ajattele roskakorissa on säilytettäviä väärä, kunnes se havaitsee lapsen ei ole yhteyteen käyttötarkoituksesta, yhteyden ei säilytetä heti.

### <a name="partitioning-support"></a>Jakaminen tuki
Lopullinen Huomaa, että Huomautus affiniteetti ei kyseisen affiniteetti suhteita ei tueta, jossa ylemmän tason osioitu. Tämä on jotakin, mitä tuetaan voi myöhemmin, mutta nyt se ei ole sallittu.

## <a name="next-steps"></a>Seuraavat vaiheet
- Jos haluat lisätietoja muiden määrittämiseen käytettävissä olevat asetukset services uloskuittaus klusterin Resurssienhallinta-määrityksiä aiheen käytettävissä [tietoja palveluiden määrittäminen](service-fabric-cluster-resource-manager-configure-services.md)
- Monesta syystä, johon ihmiset käyttävät affiniteetti, kuten rajoittaminen pieni palvelujen määrittäminen laitteiden ja yritetään koostaa palvelujen kokoelma kuormituksen tuetuista paremmin sovelluksen ryhmien avulla. Tutustu [Sovelluksen ryhmistä](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
