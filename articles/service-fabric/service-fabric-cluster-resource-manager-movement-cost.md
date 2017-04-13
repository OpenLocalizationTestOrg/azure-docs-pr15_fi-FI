<properties
   pageTitle="Palvelun kangasta klusterin Resurssienhallinta: siirto kustannusten | Microsoft Azure"
   description="Siirto kustannukset kangasta Service-palvelujen yleiskatsaus"
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

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Palvelun siirto kustannukset vaikuttavien klusterin Resurssienhallinta-vaihtoehdot
Tärkeä ottaa huomioon, kun yrität selvittää, mitä muuttuu halutaan klusterin ja pisteet ratkaisun on saavuttaa kyseisen ratkaisun kokonaiskustannukset tekijä.

Siirtäminen palvelun esiintymiä tai replikoita kustannukset suorittimen aika- ja kaistanleveyden vähintään. Tilallisten Services se kustannuksia myös olevan tilan määrää levyllä, jonka haluat luoda kopion tilan ennen vanha replikoiden suljetaan. Selvästi jälkeen haluat pienentää ratkaisu, joka Azure palvelun kangasta klusterin Resurssienhallinta tulee kanssa kustannukset. Mutta et halua ohittaa ratkaisuja, jotka merkittävästi parantaa resurssien klusterin.

Klusterin Resurssienhallinta on kahdella tavalla laskemista kustannuksia ja rajoittaminen, myös silloin, kun se yrittää hallinta klusterin mukaan sen tavoitteet. Ensimmäinen on, että klusterin Resurssienhallinta on suunniteltaessa klusterin uuden asettelun funktio laskee jokaisen siirto, joka kokoelmasta. Yksinkertainen tapauksessa Jos saat kahden ratkaisujen sama saldo yleinen lopussa (tulos) ja valitse Ota on pienin kustannus (siirtää määrä).

Tämä toimii melko hyvin. Mutta samoin kuin oletus- tai staattinen latautuu, on epätodennäköistä, että kaikki siirrot ovat yhtä monimutkaisia järjestelmän. Osa on todennäköisesti paljon kalliimpi.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Replikan Siirrä kustannukset ja huomioon otettavia seikkoja muuttaminen
Ilmoittaa kuormituksen (toisen toiminnon klusterin resurssien hallinta) voit antaa palvelun vielä raportoinnin itse miten kallista palvelu on siirrettävä tiettynä ajankohtana.

Koodi:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost on neljä tasoa: nolla, pieni, Normaali tai suuri. Nämä ovat suhteessa toisiinsa, lukuun ottamatta nolla. Nolla tarkoittaa, että siirtäminen replikan on ilmainen ja olisi ei lasketa mukaan ratkaisun kirjainarvosanan. Siirrä kustannukset suuri, se *ei* takaa, että se ei siirry, että se ei voi siirtää ellei ole hyvää syytä juuri.

![Siirrä kustannukset suosio replikoiden siirron valitseminen][Image1]

MoveCost auttaa löytämään ratkaisuja, jotka aiheuttavat yleinen vähintään häiriöitä ja on helpointa saavuttamiseksi aikana edelleen saapua vastaavat saldo. Palvelun käsite kustannukset voivat olla suhteessa monella eri tavalla. Yleisimmät tekijät laskettaessa Siirrä kustannukset ovat:

- Osavaltio tai tiedot, jotka palvelulla voit siirtää määrä.
- Asiakkaiden on katkennut kustannukset. Kustannusten siirtäminen ensisijainen replika on yleensä suurempi kuin siirtäminen toissijainen replikan kustannukset.
- Keskeyttäminen tapahtumakartoitus toiminnon kustannukset. Joitakin toimintoja tietojen tallentaa tasoa tai asiakkaan kutsun yhteydessä suoritetaan toiminnot ovat kallista. Et halua lopettaa ne, jos sinun ei tarvitse tiettyjä pilkun jälkeen. Niin toiminnon toistaminen, jonka fonttikokoon kustannukset, voit pienentää todennäköisyys, että palvelun replikan tai esiintymän siirtyvät ylöspäin. Kun toiminto on valmis, sijoita se takaisin normaaliksi.

## <a name="next-steps"></a>Seuraavat vaiheet
- Palvelun kangasta klusterin resurssin Manager käyttää arvot kulutus ja kapasiteetin klusterin hallinta. Lisätietoja arvot ja niiden määrittämisestä, tutustu [hallinta resurssin kulutus ja palvelun kangasta arvot ja lataa](service-fabric-cluster-resource-manager-metrics.md).
- Lisätietoja siitä, miten klusterin Resurssienhallinta hallitsee ja jakaa kuormituksen klusterin, tutustu [tasaamisen palvelun kangasta-klusterin](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
