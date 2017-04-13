<properties
   pageTitle="Luotettavan sivustokokoelmat | Microsoft Azure"
   description="Palvelun kangasta tilallisten services on luotettava sivustokokoelmat, joiden avulla voit kirjoittaa erittäin käytettävissä skaalattava ja pieni viive cloud-sovellukset."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Johdanto luotettava sivustokokoelmat Azure palvelun kangasta tilallisten Services-palveluissa

Luotettavan sivustokokoelmat avulla voit kirjoittaa erittäin käytettävissä skaalattava ja pieni viive cloud sovellusten aivan kuin yhteen tietokoneeseen sovellusten kirjoitettaessa. **Microsoft.ServiceFabric.Data.Collections** nimitilan luokat antaa ulos,-valmiilla kokoelmien, jotka helpottavat osavaltio automaattisesti käytettävissä. Kehittäjien on ohjelmaa vain luotettava sivustokokoelman ohjelmointirajapinnan ja anna luotettavaa sivustokokoelmat replikoitua ja paikallisen-tilan hallinta.

Luotettavan sivustokokoelmat ja muita suuren käytettävyyden tekniikoita (kuten Redis.txt, Azuren taulukkojen palvelun ja Azure jonon service) avaimen ero on, että siinä on käytettävissä paikallisesti palvelun esiintymän samalla myös tehdään erittäin käytettävissä. Tämä tarkoittaa, että:

- Kaikki lukee ovat paikallisia, joka johtaa pieni viive ja suuri siirtonopeuden lukee.
- Kaikki kirjoituksia maksamaan vähimmäismäärä verkon IOs, seurauksena on pieni viive ja suuri siirtonopeuden kirjoittaa.

![Kokoelmien kehitys kuva.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Luotettavan sivustokokoelmat Voit ajatella luonnollinen kehitys **System.Collections** luokkien: kokoelmien, jotka on suunniteltu cloud ja usean tietokoneen sovellusten määrä kasvaa monimutkaisuus kehittäjille uusi joukko. Näin ollen luotettava sivustokokoelmat ovat seuraavat:

- Replikoida: Tila muuttuu, replikoida suuren.
- Samanlainen: Tiedot säilytetään levylle varmistamiseksi vastaan suurissa katkokset (esimerkiksi palvelinkeskuksen power käyttökatkosta).
- Asynkroninen: Ohjelmointirajapinnan on asynkroninen varmistaa, että viestiketjuissa siirtyminen ei estetä kun sille IO.
- Tapahtumien: Ohjelmointirajapinnan käyttämiseen tapahtumien otetaan, jotta voit hallita useita luotettava kokoelmien palvelun helposti.

Luotettavan sivustokokoelmat on vahva yhdenmukaisuuden takaa ulos ruutuun jotta päättelypeli tietoja sovelluksen tila helpottaa.
Vahvan yhdenmukaisuuden saavutetaan varmistamalla tapahtumasarjan vahvistukset valmis vasta, kun koko tapahtuma on kirjattu replikassa, mukaan lukien ensisijainen suurimmalla äänistä tapahtuma.
Saavuttamiseksi heikkoja yhdenmukaisuuden sovellukset voivat Kuittaa takaisin asiakkaan/pyytäjä ennen asynkroninen Vahvista palauttaa.

Luotettavan sivustokokoelmat-ohjelmointirajapinnan on kehitys samanaikainen kokoelmien API (löytyy **System.Collections.Concurrent** nimitilan):

- Asynkroninen: Palauttaa tehtävän, koska toisin kuin samanaikainen sivustokokoelmat toimintojen replikoida ja samanlainen.
- Ei, parametrit: käyttää `ConditionalValue<T>` palauttaa totuusarvoksi ja arvon sijaan ulos parametrit. `ConditionalValue<T>`on kuin `Nullable<T>` , mutta ei edellytä on struct T.
- Tapahtumien: Käyttää tapahtumaobjektin käyttäjälle oikeuden useita luotettava sivustokokoelmat tapahtumassa ryhmän toiminnot.

Nykyään **Microsoft.ServiceFabric.Data.Collections** on kaksi:

- [Luotettavan sanaston](https://msdn.microsoft.com/library/azure/dn971511.aspx): edustaa replikoitua, tapahtumien ja asynkroninen kokoelma avain/arvo parit. Kaltaisilta **ConcurrentDictionary**avain ja arvo voi olla mikä tahansa.
- [Luotettavan jonossa](https://msdn.microsoft.com/library/azure/dn971527.aspx): edustaa replikoitua, tapahtumien ja asynkroninen tarkka ensimmäisen sisään, tarkoitettua (FIFO) jonossa. Kaltaisilta **ConcurrentQueue**arvo voi olla mikä tahansa.

## <a name="isolation-levels"></a>Eristystaso tasot
Eristystaso määritetään aste, johon tapahtuma on oltava eristetty muiden tapahtumien tekemät muutokset.
On kaksi eristystaso tasoa, joita tuetaan luotettava sivustokokoelmat:

- **Toistettava luku**: määrittää, että lauseet ei voi lukea tietoja, joita on muokattu, mutta ei vielä muut tapahtumat ja muiden tapahtumien voit muokata tietoja, jotka on luettu suljettavan, kunnes nykyinen tapahtuma loppuu. Lisätietoja on artikkelissa [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
- **Tilannevedoksen**: määrittää olevan lukea minkä tahansa lauseen tapahtuman tiedot muulla tavoin yhdenmukaisia version tiedot, jotka olivat tapahtuman alkuun.
Tapahtuman voi tunnistaa vain tietojen muutokset, jotka on toteutettu ennen tapahtuman alkua.
Muiden tapahtumien nykyisen tapahtuman alkamisen jälkeen tekemät tietojen muutokset eivät näy lauseet nykyisen tapahtuman suorittamisen.
Vaikutus on aivan kuin tapahtuman lauseet saat tilannevedoksen vahvistettu tiedot tapahtuman alkuun mukaisessa muodossa.
Tilannevedoksia ovat yhdenmukaisia luotettava sivustokokoelmat.
Lisätietoja on artikkelissa [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Luotettavan sivustokokoelmat Valitse automaattisesti eristystaso tietyn lukemiseen toiminnan toiminto ja se roolin mukaan tapahtuman luomisen aikana käytettävät.
Seuraavassa on taulukko, joka esittää eristystaso tason oletusarvojen luotettava sanasto ja jonon toiminnot.

| Toiminnon \ rooli      | Ensisijainen          | Toissijainen        |
| --------------------- | :--------------- | :--------------- |
| Yhden kohteen luku    | Toistettava luku  | Tilannevedos         |
| Luettelointi \ määrä   | Tilannevedos         | Tilannevedos         |

>[AZURE.NOTE] Yleisiä esimerkkejä yksittäisen kohteen toiminnot ovat `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

Luotettavan sanasto ja luotettava jonossa tukevat luku Your kirjoittaa.
Toisin sanoen minkä tahansa Kirjoita tapahtumasta näkyvät seuraavat luetaan, joihin kuuluu saman tapahtuman.

## <a name="locking"></a>Lukitseminen
Luotettavan sivustokokoelmat kaikki tapahtumat ovat kaksi aikajaksotettu: tapahtuman vapauta se on hankittu, kunnes tapahtuman lopettaa keskeytys tai vahvistamista lukituksia.

Luotettavan sanaston käyttää rivin tasoa lukitseminen yhden kohteen kaikki toiminnot.
Luotettavan jonon maatalousyrityksessä samanaikainen tarkka tapahtumien FIFO-ominaisuuden käytöstä.
Luotettavan jonon käyttää toimintoa tason lukitukset salliminen tapahtuman, jonka `TryPeekAsync` ja/tai `TryDequeueAsync` ja yksi tapahtuma, jonka `EnqueueAsync` kerrallaan.
Huomaa, että voit säilyttää FIFO, jos `TryPeekAsync` tai `TryDequeueAsync` tasapuolisesti koskaan luotettava jono on tyhjä, ne myös lukittuu `EnqueueAsync`.

Kirjoita toiminnot kestävät aina poissulkeva lukitukset.
Lue toimille lukitusta riippuu siitä, pari tekijät.
Mikä tahansa luku toiminto valmis käyttämällä tilannevedoksen eristystaso on lukittu vapaa.
Oletusarvon mukaan mitään toistettava luku-toimintoa on jaettu lukitukset.
Kuitenkin, Lue toiminnon, joka tukee toistettava luku, käyttäjä voi pyytää päivitys-Lukitse jaettu Lukitse sijaan.
Päivityksen lock on julkiseen Lukitse, joka estää yhteinen lomake, joka seuraa, kun useita tapahtumia Lukitse resurssien mahdolliset päivitykset voi tehdä myöhemmin lukituksen avulla.

Lukitse yhteensopivuuden matriisin löytyvät alla:

| Pyydä \ myöntää | Ei mitään         | Jaettu       | Päivitys      | Yksityisesti    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Jaettu            | Ei ole ristiriita  | Ei ole ristiriita  | Ristiriita    | Ristiriita     |
| Päivitys            | Ei ole ristiriita  | Ei ole ristiriita  | Ristiriita    | Ristiriita     |
| Yksityisesti         | Ei ole ristiriita  | Ristiriita     | Ristiriita    | Ristiriita     |

Huomaa, että aikakatkaisu-argumentti on luotettava sivustokokoelmat-ohjelmointirajapinnan käytetään lukituksen tunnistus.
Esimerkiksi kaksi tapahtumaa (T1 ja T2) rooliin lukea ja päivittää K1.
On mahdollista niiden Umpikuja, koska ne lopputulos on jaettu lukitus.
Tässä tapauksessa jommankumman tai molempien toimintojen aikakatkaistaan.

Huomaa, että edellä lukituksen skenaarion on erinomainen esimerkki siitä, miten päivitys lukitseminen estää lukkiutuu.

## <a name="persistence-model"></a>Pysyvyyttä malli
Luotettavan tilan hallinta- ja luotettava sivustokokoelmat noudattamalla pysyvyyttä mallin, jota kutsutaan Log ja tarkistuspiste.
Tämä on malli, jossa muuttuessa tilassa on kirjautunut levyn ja käytetään vain muistissa.
Valmis tilan itse säilytetään vain toisinaan (tietovälinettä Tarkistuspiste).
On se etu, että deltas on otettu huomioon peräkkäisiä vain liittäminen-kirjoituksia levyllä parannettu suorituskyky.

Ymmärtämään Log ja tarkistuspiste mallia, ensin katsotaan äärettömän levy-skenaario.
Luotettavan tilan hallinta kirjaa jokaisen toiminnon, ennen kuin se on replikoida.
Näin voit käyttää vain toiminnon muistissa luotettava sivustokokoelman.
Koska lokit säilyvät, vaikka se ei onnistu, ja se on käynnistettävä, luotettava tilan hallinta on tarpeeksi tietoa sen lokit Toista kaikki toiminnot, se on katkennut.
Levy on äärettömän, lokitiedostoon tarvitse koskaan poistetaan ja luotettava sivustokokoelman on ladatun-tilan hallinta.

Nyt katsotaan rajallinen levy-skenaario.
Kun lokitiedostoon saavutustasopisteet-luotettavia tilan hallinta sivustokokoelmiesi levytilaa.
Avautuvasta luotettava tilan hallinta on tilaa uudempaan tietueiden lokitiedostonsa.
Se pyytää niiden ladatun tilaan levylle, tarkistuspiste luotettava sivustokokoelmat.
Se on luotettava sivustokokoelmat vastuu jatkuvat tilaan ylöspäin kyseiseen pisteeseen.
Kun luotettava sivustokokoelmat valmis niiden tarkistuspisteet, luotettava tilan hallinta voit Katkaise kirjaa Vapauta levytilaa.
Tällä tavalla, kun se on käynnistettävä, luotettava sivustokokoelmat palauttaa niiden checkpointed tilaan ja luotettava tilan hallinta palauttaminen ja toistaminen kaikki tilan muutokset, jotka tarkistuspiste jälkeiset.

>[AZURE.NOTE] Checkpointing, Lisää toinen arvo on, että se tehostaa palautus yleisiä tapauksissa.
Tämä johtuu siitä tarkistuspisteet sisältää uusimmat versiot.

## <a name="recommendations"></a>Suosituksia

- Älä muokkaa mukautettuja tyypin luettujen palauttama objektin (esimerkiksi `TryPeekAsync` tai `TryGetValueAsync`). Luotettavan sivustokokoelmat, aivan kuten samanaikaisia sivustokokoelmat palauttaa viittauksen objektit ja kopion.
- Muuta laaja kopioi mukautetun tyypin palautettu objekti ennen sen muokkaamista. Koska Struct ja sisäiset tyypit pass arvon, ei tarvitse tehdä niihin laaja kopio.
- Älä käytä `TimeSpan.MaxValue` aikakatkaisu varten. Aikakatkaisu käytetään esiintyvien lukkiutuu.
- Älä käytä tapahtumaa, kun se on vahvistettu, keskeytetty tai poistettu.
- Älä käytä luettelointi se on luotu tapahtuma-alueen ulkopuolella.
- Älä luo toinen tapahtumasta tapahtuman `using` lauseen, koska se voi aiheuttaa lukkiutuu.
- Varmista, että `IComparable<TKey>` toteutus on oikea. Järjestelmä kestää riippuvuuden tästä tarkistuspisteet yhdistämistä varten.
- Käytä päivityksen Lukitse luettaessa kohteen, jonka tarkoitus päivittää sen luokan lukkiutuu estämiseksi.
- Harkitse varmuuskopiointi ja palauttaminen toiminnot, jotka on palauttaminen.
- Vältä yhden kohteen toiminnot ja usean kohteen toimintojen sekoittamista keskenään (esimerkiksi `GetCountAsync`, `CreateEnumerableAsync`) saman tapahtuman vuoksi eri eristystaso tasot.

Seuraavat asiat kannattaa ottaa huomioon:

- Oletus-aikakatkaisun on luotettava sivustokokoelman ohjelmointirajapinnan neljä sekuntia. Useimmat käyttäjät eivät korvaa tämän.
- Oletusarvoinen peruutus tunnus on `CancellationToken.None` kaikki luotettava sivustokokoelmat API.
- Luotettavan sanaston avaimen tyyppi-parametrin (*TKey*) on pantava täytäntöön oikein `GetHashCode()` ja `Equals()`. Näppäimet on oltava pysyvä.
- Saavuttamiseksi suuren käytettävyyden luotettava kokoelmien kunkin palvelun pitäisi olla vähintään yksi kohde ja pienin replikajoukon 3 kokoa.
- Toissijaisen toimenpiteet luku voi lukea versioita, joita ei ole vahvistettu quorum.
Tämä tarkoittaa, että tiedot, jotka on luettu yhden toissijaisen versio voi olla EPÄTOSI edennyt.
Lukuja, valitse ensisijainen ovat aina vakaata: Voit aina oltava false edennyt.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Luotettavan palvelut-pikaopas](service-fabric-reliable-services-quick-start.md)
- [Luotettavan Kokoelmien käyttäminen](service-fabric-work-with-reliable-collections.md)
- [Luotettavan palveluiden ilmoitukset](service-fabric-reliable-services-notifications.md)
- [Luotettavan palveluiden varmuuskopiointi ja palauttaminen (palauttaminen)](service-fabric-reliable-services-backup-restore.md)
- [Luotettava tilan hallinnan määrittäminen](service-fabric-reliable-services-configuration.md)
- [Palvelun kangasta verkko-Ohjelmointirajapinnan palveluiden käytön aloittaminen](service-fabric-reliable-services-communication-webapi.md)
- [Laajennettu luotettava palveluihin ohjelmointi mallia](service-fabric-reliable-services-advanced-usage.md)
- [Luotettavan loppu Sovelluskehittäjän opas](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
