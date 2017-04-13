<properties
   pageTitle="Tasaamisen yhteyttä klusterin kanssa Azure palvelun kangasta klusterin Resurssienhallinta | Microsoft Azure"
   description="Johdanto tasaamisen yhteyttä klusterin palvelun kangasta klusterin Resurssienhallinta."
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

# <a name="balancing-your-service-fabric-cluster"></a>Tasaamisen palvelun kangasta-klusterin
Palvelun kangasta klusterin Resurssienhallinta avulla dynaaminen kuormitus raportointi, muutokset klusterin reagoi, rajoituksen virheitä korjataan ja yhdistelemällä klusterin tarvittaessa. Mutta miten usein se tee seuraavat asiat ja mitä käynnistää sen? On useita ohjausobjekteja tähän liittyviä.

Ohjausobjektien ympärille tasaamisen ensimmäisiin on muutamia ajastimet. Näiden määrittävät, kuinka usein klusterin Resurssienhallinta tutkii asioita, jotka on pystyttävä vastaamaan klusterin tila. On kolme eri luokkia tehdyn työn, joissa omia vastaavan ajastin. Ne:

1.  Asettelun – tässä vaiheessa käsitellään sijoittaminen tahansa tilallisten replikoiden tai tilattomien esiintymät, joka ei näy. Tämä koskee uusia palveluja ja käsittelemisen tilallisten replikoiden tai tilattomien esiintymät, joka on epäonnistui ja täytyy ehkä luoda uudelleen. Poistaminen ja pudottaminen replikoiden tai esiintymät käsitellään myös tässä.
2.  Rajoitus tarkistaa – tässä vaiheessa tarkistaa ja korjaa, jos järjestelmässä eri sijaintia rajoituksia (säännöt). Säännöt ovat esimerkiksi varmistaa, että solmujen eivät kapasiteetin päälle ja palvelun sijainti rajoituksia (Lisätietoja näistä uudempi) täyttyvät.
3.  Tasaamisen – tässä vaiheessa tarkistaa ennakoiva yhdistelemällä on tarpeen mukaan eri arvot saldo määritetyn haluamasi taso ja jos yrittää, Etsi lajittelujärjestyksen klusterin, joka lisää täsmätään.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Klusterin Resurssienhallinta vaiheet ja ajastimet määrittäminen
Kaikki nämä korjaukset klusterin Resurssienhallinta tehdä erityyppisiä ohjaa eri ajastin, joka ohjaa sen korkojakso. Esimerkiksi jos haluat vain käsitellä siten uusi palvelu työmääriä klusterin jokaisen tunnin (erä ne ylöspäin), mutta haluat tarkistaa säännöllisesti tasaamisen muutaman sekunnin välein, voit määrittää toiminnon. Kunkin ajastin käynnistyy, kun tehtävä on ajoitettu. Oletusarvoisesti Resurssienhallinta tarkistaa sen ja koskee päivitykset (jonottaminen kaikki muutokset, joita viimeisen tarkistuksen kuten huomaamatta solmu on alaspäin jälkeiset) jokaisen 1/10 sekunnin, määrittää sijaintia ja rajoitus tarkistaa liput sekunnin välein ja täsmäyttäminen Merkitse viiden sekunnin välein.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Tänään on vain käyttämällä jotakin näistä toiminnoista kerrallaan, peräkkäin (minkä vuoksi-tunnukseksi määritysten kuin "pienin väliajoin")). Tämä on niin, että esimerkiksi olemme olet jo vastannut luoda uusia replikoita ennen on siirtyä tasaamisen klusterin odotetaan pyynnöt. Kuten näet määritetty oletusarvoinen aikajaksoittain, emme voi tarkistaa ja tarkista mitään annettava hyvin usein, mikä tarkoittaa, että muutokset on Soita jokaisen vaiheen lopussa on yleensä pienempi: emme ei scanning kautta tuntia klusterin tehdyistä muutoksista ja yrittää korjata ne kaikki kerralla, emme rooliin käsittelee asioita enemmän tai vähemmän päivittyvät näyttöön reaaliajassa mutta jonottaminen, kun monella eri tavalla käydä osoitteessa samaan aikaan. Näin palvelun kangasta resurssien hallinnan hyvin vastaa toimintoja, jotka tapahtuvat klusterin.

Useimmat näistä aikana on helppoa, (Jos rajoitus virheitä, korjaa, jos käytettävissä on services luodaan, luo niitä on), klusterin Resurssienhallinta on myös muita tietoja, onko imbalanced klusterin. Jossa on kaksi määritysten laitteita: *Tasaamisen raja-arvot* ja *Tehtävän raja-arvot*.

## <a name="balancing-thresholds"></a>Tasaamisen raja-arvot
Tasaamisen raja-arvo on ensisijainen ohjausobjektin käynnistävä ennakoiva yhdistelemällä (muista, että ajastin on juuri, kuinka usein klusterin Resurssienhallinta Tarkista – se ei tarkoita mitään, mitä tapahtuu). Tasaamisen raja-arvo määrittää, miten imbalanced klusterin on oltava tietyn mittarin järjestyksessä, klusterin Resurssienhallinta huomioitavia se imbalanced ja käynnistin tasaamisen.

Vastatilin raja-arvot on määritetty osana klusterin määritelmän metrijärjestelmä kohti välein. Lisätietoja [tämän artikkelin](service-fabric-cluster-resource-manager-metrics.md)Katso arvot.

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Tasaamisen kynnysarvo mittarin on suhde. Jos vähintään ladattu solmu kuormituksen määrä jaettuna viimeksi ladattu solmu kuormituksen määrä ylittää tämän numeron, valitse klusterin pidetään imbalanced ja tasaamisen käynnistyy seuraavan kerran klusterin Resurssienhallinta tarkistaa (oletusarvoisesti koskaan 5 sekuntia kuin säännelty mukaan MinLoadBalancingInterval, kuten alla).

![Tasaamisen raja-Esimerkki][Image1]

Yksinkertainen tässä esimerkissä kunkin palvelun on käyttö joidenkin metrijärjestelmä yhden yksikön. Ylimmät esimerkissä solmun suurimman kuormituksen on 5 ja pienin arvo on 2. Oletetaan, että tämä arvo Vastatilin raja-arvo on 3. Tämän vuoksi yläreunan esimerkissä klusterin pidetään saapuva ja ei ole tasaamisen käynnistyy, kun klusterin Resurssienhallinta tarkistaa (koska klusterin suhde on 5/2 = 2,5 ja, joka on pienempi kuin määritetty Vastatilin raja 3).

Ala-esimerkissä solmu max kuormitus on 10 ollessa vähintään 2, tuloksena on suhde on 5. Tämä siirtää klusterin suunnitellun Vastatilin raja-arvon, 3, metrijärjestelmän päälle. Tuloksena yleinen rebalancing Suorita on ajoitettu seuraavan kerran Vastatilin ajastin käynnistyy. Huomaa, että vain Vastatilin haku on poistettu ei tarkoita mitään siirtyvät – joskus klusterin on imbalanced, mutta tilanne entistä parempi - mutta katsomalla tilanne (vähintään oletusarvon mukaan) joitakin Lataa jaetaan lähes varmasti Node3. Huomaa, että koska emme Käytä alho lähestymistapa joitakin kuormituksen voi myös jaetaan Node2 jälkeen, joka johtaa vesistöihin solmujen yleinen eroista, mutta on todennäköisesti odottanut, että suurin osa kuormituksen flow Node3.

![Tasaamisen kynnysarvo Esimerkki toiminnot][Image2]

Huomautus, että käytön alla Vastatilin raja-arvo ei ole eksplisiittinen tavoitteen – tasaamisen raja-arvot ovat vain *käynnistin* , joka kertoo klusterin Resurssienhallinta, se pitäisi näyttää määrittämään mitä päivityksiä, voit tehdä, jos sellainen on klusterin kyselyjä.

## <a name="activity-thresholds"></a>Tehtävän raja-arvot
Joskus, vaikka solmujen suhteellisen imbalanced, *kuormituksen klusterin kokonaismäärä* on pieni. Tämä voi johtua vain kellonaika tai koska klusterin on uusia ja vain hakeminen bootstrapped. Kummassakin tapauksessa ei kannattaa käyttää aikaa tasaamisen klusterin, koska on todella hyvin vähän voidaan – sinun vain voidaan yksinkertaistaa verkko- ja Laske resurssien asioita ympärille, siirrä ilman, että suoria erot. Koska haluamme välttää näin, on toiseen ohjausobjektiin sisältyy Resurssienhallinta, nimeltään tehtävän raja-arvot, joiden avulla voit määrittää joitakin suora alaraja tehtävälle – Jos ei ole solmu on vähintään tämä paljon kuormituksen sitten tasaamisen ei käynnistyy vaikka tasaamisen kynnysarvo täyttyy.

Esimerkkinä oletetaan, että on nämä solmuissa kulutus seuraavat kokonaissummat raportit. Oletetaan, että olemme säilyttää Microsoftin tasaamisen kynnysarvo, tämä arvo 3, mutta nyt myös on tehtävä-kynnysarvo 1536, myös. Ensimmäinen tapauksessa klusterin ollessa imbalanced kohti tasaamisen raja-arvo ei ole solmu täyttää kyseisen tehtävän alaraja, niin jäädä asioita yksin. Ala-esimerkissä Node1 ylittäneen tapa tehtävän raja-arvon, jotta tasaamisen suoritetaan, (koska tasaamisen raja-arvon ja tehtävän raja arvo on ylitetty)

![Tehtävän raja-Esimerkki][Image3]

Tavoin tasaamisen raja-arvot tehtävän raja-arvot ovat määritetyn kohti kuin metrijärjestelmän klusterin määritelmän kautta:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Huomaa, että tasaamisen ja tehtävän raja-arvot ovat molemmat sidottu arvo - tasaamisen vain käynnistyy Jos tasaamisen ja tehtävän raja-arvot ovat ylittivät saman metrijärjestelmä. Näin, jos emme ole tasaamisen raja-arvoa muistin ja toimintojen kynnysarvo suorittimen, tasaamisen ei käynnistää, kunhan jäljellä olevan raja-arvot (suorittimen tasaamisen kynnysarvo) ja muistin tehtävän raja-arvo ei ole ylitetty.

## <a name="balancing-services-together"></a>Tasaamisen services yhdessä
Kohdetta, joka ei kiinnostavat Huomaa on onko klusterin imbalanced on päätös klusterin laajuinen, mutta tapaa, jolla on Siirry tietoja se siirtyy yksittäisten palvelujen replikoita ja esiintymät ympärille. Tämä on järkevää, oikea? Jos muisti on pinottu jonkin solmun useita replikoita tai se saattaa aiheuttaa esiintymät, niin se voi vaatia siirtäminen tilallisten replikoiden tai tilattomien esiintymät, jotka käyttävät kyseessä olevaan, imbalanced-arvo.

Joskus mutta asiakkaan soittavat us ylös tai tiedoston lippu ilmoittaa, että palvelu, jota ei ole imbalanced siirtyi. Miten voi ongelmassa, että palvelu saa siirtää, vaikka kaikki kyseisen palvelun arvot saapuva, vaikka täysin niin muiden ristiriidan milloin? Katsotaan!

Ota esimerkiksi neljä services, Service1, Service2, Service3 ja Service4. Service1 raportit mittaukset Metric1 ja Metric2, Service2 Metric2 ja Mmetric3 Service3 Metric3 ja Metric4 ja Service4 vastaan joitakin metrisillä Metric99 vastaan. Jos teet näet, jossa tutustutaan tähän. Yhdistettyjen on! Näkökulmasta klusterin Resurssienhallinta on todella ei ole neljä riippumaton services-palvelut, jotka liittyvät (Service1 Service2 ja Service3) ja se, joka ei ole käytössä, kun yksinään useita on.

![Tasaamisen Services yhdessä][Image4]

Se on mahdollista Metric1 ristiriidan heikentää replikoiden tai kuuluvat eri osat Service3 esiintymät. Yleensä kyseisissä on melko rajoitettu, mutta voit olla suurempi sen mukaan, miten imbalanced Metric1 täsmälleen käytössä ja muutokset oli tehtävä klusterin, jotta voit korjata. On myös sanomalla varmuudella arvot 1, 2 tai 3 ristiriidan koskaan aiheuttaa liikkeitä Service4 – jälkeen siirtää replikat ilmaiseminen olisi ei ole tai kuuluvat Service4 can ympärillä tapauksia jätä ehdottoman vaikuttaa arvot 1, 2 tai 3 saldo.

Klusterin Resurssienhallinta esiintyy automaattisesti, mitä palveluja liittyvät, koska palvelut mahdollisesti lisättyjä, poistettu, tai oli niiden metrisillä määritysten muutos – esimerkiksi, kaksi peräkkäisiä tasaamisen Service2 välillä on voitu määrittää Poista Metric2. Tämä katkaisee ketju Service1 ja Service2 välillä. Nyt sen sijaan, että kaksi ryhmää palveluista, sinulla on kolme:

![Tasaamisen Services yhdessä][Image5]

## <a name="next-steps"></a>Seuraavat vaiheet
- Arvot ovat, miten palvelun kangasta klusterin resurssin Manager hallitsee kulutus ja kapasiteettiin klusterin. Saat lisätietoja niitä ja miten ne määritetään Tutustu [tämän artikkelin](service-fabric-cluster-resource-manager-metrics.md)
- Siirto kustannus on yksi tapa Signalointi klusterin Resurssienhallinta tietyt palvelut ovat kalliimpi Siirrä kuin muut. [Tässä artikkelissa](service-fabric-cluster-resource-manager-movement-cost.md) viitata lisätietoja siirto kustannukset
- Klusterin Resurssienhallinta on useita, voit määrittää churn klusterin hidastaa ylikuormitustilan. Ne eivät yleensä ole tarpeen, mutta jos tarvitset niitä on lisätietoa heistä [tähän](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
