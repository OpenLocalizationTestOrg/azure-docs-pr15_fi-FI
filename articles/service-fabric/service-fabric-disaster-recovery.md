<properties
   pageTitle="Azure palvelun kangasta palauttaminen | Microsoft Azure"
   description="Azure palvelun kangasta on tarpeen kaikentyyppisissä lieventämiseksi ominaisuuksia. Tässä artikkelissa tiedostotyypit, joka voi edetä ympäristökatastrofien ja miten voit käsitellä niitä."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Valitse Azure palvelun kangasta palauttaminen

Tärkeä osa välittää suuren käytettävyyden cloud-sovellus on varmistaa, että se voidaan uutta kaikenlaisia virheet, mukaan lukien ohjausobjektin ulkopuolella olevat käyttöä varten. Tässä artikkelissa kuvataan Azure palvelun kangasta-klusterin mahdolliset lieventämiseksi kontekstissa rakenteesta ja ohjeita siitä, miten voit käsitellä tällaisia lieventämiseksi rajoittaa tai käyttökatkot tai tietojen menettämisen riskiä poistamiseksi.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Palvelun kangasta rakenteesta klusterit Azure-tietokannassa

Erityyppiset virheet aiheuttaman ymmärretty, on hyvä tietää, miten klustereiden ovat fyysisesti korttimuodossa Azure.

Kun luot palvelun kangasta klusterin Azure, tarvitaan Valitse alue, jossa se isännöidään. Azure infrastruktuurin valmistelee sitten resurssit, klusterin alueella kuvatyyppien näennäiskoneiden (VMs), jotka on pyydetty määrä. Katsotaan tarkemmin miten ja missä ne VMs valmistelun yhteydessä.

### <a name="fault-domains"></a>Vian toimialueet

Oletusarvon mukaan klusterin VMs leviävät tasaisesti looginen ryhmien nimeltään vika domains (FDs), joka määritetään koneet mahdolliset virheet Host (isäntä)-laitteiden perusteella. Erityisesti jos kaksi VMs sijaitsevat kaksi eri FDs, voit olla varma, että ne Jaa samaan power lähde- tai vaihda. Tuloksena power virheen vaikuttavia yhden AM tai paikalliseen verkkoon kirjauduttaessa ei vaikuta muihin, sallimisen palvelun kangasta, saattaa jälleen tasapainoon ei vastaa tietokoneen sisällä klusterin kuormitus.

Voit havainnollistaa yhteyttä klusterin asettelun vika toimialueilla annettu [kangasta](service-fabric-visualizing-your-cluster.md)Explorerissa klusterin kartan avulla:

![Kerro vika toimialueilla kangasta Explorerissa solmujen][sfx-cluster-map]

>[AZURE.NOTE] Muut akselin klusterin määrityksen näyttää päivityksen toimialueita, mihin ryhmään loogisesti solmujen suunnitellun ylläpidon tehtävien perusteella. Palvelun kangasta klustereiden Azure-tietokannassa on aina aseteltu viisi päivityksen toimialueilla.

### <a name="geographic-distribution"></a>Maantieteelliset jakauman.

Tällä hetkellä [26 Azure alueiden kaikkialla maailmassa][azure-regions], on useita Lisää ilmoitettiin. Yksittäisten alueen voi olla vähintään yksi tiedot keskukset demand ja sopiva sijainnit muiden tekijöiden kesken käytettävyyttä mukaan. Huomaa kuitenkin, että myös alueet, jotka sisältävät useita tiedot keskikohdan mukaan-ei ei takaa, että yhteyttä klusterin VMs tasaisesti jakamisen fyysinen sijaintien välillä. Myös tällä hetkellä kaikki VMs annetun klusterin valmistellaan yksittäisen fyysinen sivustossa.

## <a name="dealing-with-failures"></a>Virheiden käsittely

On erilaisia virheitä, jotka vaikuttavat klusterin jokaisella on oma rajoituksen. Olemme näyttää niitä ilmenee todennäköisyys järjestyksessä.

### <a name="individual-machine-failures"></a>Yksittäisten machine-virheet

Kuten edellä, virheet, AM tai laitteiston tai ohjelmiston isännöinnin vika toimialueen yksittäisiä koneen riskin ei yhteiskäyttöä. Palvelun kangasta yleensä havaitsee virheen muutamassa ja vastata vastaavasti klusterin tilan perusteella. Esimerkiksi jos solmu on isännöinnin ensisijainen replikoiden osion uusi ensisijainen on valittuna kohteesta osion toissijainen replikoita. Kun Azure tuo epäonnistui koneen Varmuuskopioi, se liittyä klusterin automaattisesti ja käyttää uudelleen sen osuus työmäärää.

### <a name="multiple-concurrent-machine-failures"></a>Useita samanaikaisia machine-virheet

Kun vika toimialueiden vähentää huomattavasti samanaikainen koneen virheet, tehtäviä on aina mahdollisten useita satunnainen virheitä, voit tuoda useita koneet klusterin alaspäin samanaikaisesti.

Yleensä useimpia solmut ovat kuitenkin käytettävissä, kun klusterin säilyvät toimia, vaikka osoitteessa alemman kapasiteetin tilallisten replikoiden Hae pakattu koneet pienempi joukkoon ja vähemmän tilattomien esiintymät ovat käytettävissä levittämällä kuormituksen.

#### <a name="quorum-loss"></a>Quorum tappio

Jos replikat tilallisten palvelun osion useimpia siirtyä, kyseisen osion siirtyy tilaan nimeltään "quorum tappio." Tässä vaiheessa palvelun kangasta lopettaa salliminen kirjoituksia kyseisen osion varmistaa, että sen tila on yhdenmukaista ja luotettavaa. Olemme valitsemassa, Hyväksy ajan varmistaa, että asiakkaat ovat ei kertoo, että sen tiedot on tallennettu ollessaan itse asiassa ei ole käytettävissä. Huomaa, että olet osallistuvat salliminen lukee toissijaisen replikoiden tilallisten palvelun poistaminen, voit edelleen suorittavan ne, lue tämä tila-toimintoja. Osion pysyy quorum menettämisen, kunnes riittävästi replikamäärälle palata tai klusterin järjestelmänvalvojaan pakottaa järjestelmän Siirrä [Korjaa ServiceFabricPartition Ohjelmointirajapinnan]käyttäminen[repair-partition-ps].

>[AZURE.WARNING] Tietojen menetyksen johtaa suorittamiseen korjaa-toiminto, kun ensisijainen se ei ole käytettävissä.

Järjestelmäpalveluita voi olla myös quorum tappio parhaillaan palvelukohtaisia poistettavaan vaikuttavaa. Quorum tappio nimeämiskäytäntö palvelussa vaikuttaa esimerkiksi nimenselvitys, olisi quorum tappio automaattisesti hallintapalvelu estää uusia palvelun luomisen ja failovers. Huomaa, että toisin kuin Omat Services yrittää korjata järjestelmäpalveluita ei *ole* suositeltavaa. Sen sijaan on suositeltavaa yksinkertaisesti Odota, kunnes alaspäin replikoiden palauttaa.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>Quorum menettämisen riskiä pienentäminen

Voit pienentää quorum menettämisen riskiä kasvattamalla kohde replikan määrittäminen palvelulle. Kannattaa ajatella määrä on, sinun on myös kun jäljellä käytettävissä kirjoituksia, mutta säilyttämällä että sovelluksen hyväksyt osoitteessa ei ole käytettävissä solmujen määrän tai klusterin päivityksiä voi tehdä solmujen tavoitettavissa laitteisto lisäksi.

Harkitse seuraavien esimerkkien olettaen, että olet määrittänyt palvelujen on kolme suositus tuotannon services pienimmän luvun MinReplicaSetSize. Kolme TargetReplicaSetSize kanssa (yksi ensisijainen ja kaksi secondaries) quorum tappio aiheuttaa epäonnistui päivityksessä (replikat alaspäin) ja palvelun muuttuu vain luku-tilassa. Vaihtoehtoisesti jos sinulla on viisi replikoiden, sinun voi kestävät kaksi virheet (kolme replikoiden alas) päivityksen aikana, kun muut replikat voit edelleen lomakkeen pienin replikaryhmässä asetettu vaatimus.

### <a name="data-center-outages-or-destruction"></a>Tietoja center katkokset tai hävittämisestä

Vain harvoin tiedot keskikohdan mukaan voi olla katkennut power tai verkon menettäminen. Tällaisissa tapauksissa palvelun kangasta klustereiden ja sovellusten samoin poistetaan käytöstä, mutta tiedot säilyvät. Saat klustereiden Azure käynnissä, voit tarkastella päivitykset katkokset [Azure tila-sivun][azure-status-dashboard].

Koko fyysinen tietokeskuksen tuhotaan erittäin epätodennäköistä tapauksessa kaikki palvelun kangasta klustereiden isännöidään siellä menetetään, sekä niiden tilaan.

Voit suojautua tämä mahdollisuus on vakavasti tärkeitä säännöllisesti geo tarpeettomat store [Varmuuskopioi osavaltio](service-fabric-reliable-services-backup-restore.md) ja varmistaa, että tarkistaneet voi palauttaa. Kuinka usein varmuuskopiointia on riippuvainen palautus pisteen tavoitteen (RPO). Vaikka sinulla ei ole täysin toteutettu varmuuskopiointi ja palauttaminen vielä, olisi Toteuta käsittelijä varten `OnDataLoss` tapahtuman niin, että voit kirjautua, kun se esiintyy seuraavasti:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Ohjelmistovirheet ja tietojen menettämistä muista lähteistä

Tietojen menetyksen syy-koodin häiriöitä services, ihmisten toiminnallisia virheitä ja suojauksen ilmeisesti ovat tavallisimmista kuin juuri data Centerissä virheet. Kaikissa tapauksissa palauttamisen strategia on kuitenkin sama: Ota kaikki tilallisten palvelujen säännöllisen varmuuskopioinnin suunnittelu ja syytä olla järjestelmänvalvoja palauttaa valtion.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue, miten voit simuloida [testattavuus framework](service-fabric-testability-overview.md) käyttämällä eri virheet
- Lue tietojen palauttaminen ja suuren käytettävyyden resursseihin. Microsoft on julkaissut suuria määriä ohjeet näiden aiheista. Jotkin tiedostot viittaa tietyn tekniikoita käytettäväksi muissa tuotteissa, kun ne sisältävät useita yleisiä parhaita käytäntöjä, jotka voit käyttää myös palvelun kangasta yhteydessä:
 - [Käytettävyys tarkistusluettelo](../best-practices-availability-checklist.md)
 - [Tietojen palauttaminen tietojen noutamista](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Tietojen palauttaminen ja Azure sovellusten suuri käytettävyys][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
