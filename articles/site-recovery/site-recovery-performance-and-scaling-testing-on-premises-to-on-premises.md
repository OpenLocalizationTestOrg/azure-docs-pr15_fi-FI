<properties
    pageTitle="Testi ja skaalausta tulokset paikallisen sivuston palauttaminen paikallisen Hyper-V replikointia | Microsoft Azure"
    description="Tässä artikkelissa on tietoja paikalliseen paikallisen replikointi käyttämällä Azure sivuston suorituskykyä."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Suorituskyvyn testaaminen ja Skaalaa tulokset paikalliset paikallisen Hyper-V replikointi ja palauttaminen

Voit käyttää Microsoft Azure sivuston palauttaminen orchestrate ja hallita replikoinnin näennäiskoneiden ja fyysinen palvelimien Azure tai toissijainen palvelinkeskukseen. Tässä artikkelissa on suorituskyvyn testaus on ollut, kun replikoiminen Hyper-V näennäiskoneiden kahden paikallisen palvelinkeskusten tulokset.



## <a name="overview"></a>Yleiskatsaus

Testaus tavoitteena on tutkia, miten Azure palauttaminen suorittaa muuttumattoman tilan replikoinnin aikana. Muuttumattoman tilan Replikointi tapahtuu, kun näennäiskoneiden suorittanut ensimmäisen replikoinnin ja synkronoida delta muutokset. On tärkeää käyttämällä muuttumattoman tila, koska se on tila, johon useimmat näennäiskoneiden pysyvät, ellei odottamattomia katkokset ilmetä suorituskykyä.


Testi-käyttöönoton muodostui kahden paikallisen sivuston sivustosta VMM palvelimen kanssa. Testi-käyttöönoton on tyypillinen pään office/haaran office-käyttöönotto pään visuaalisessa muodossa ensisijainen sivuston ja haaran office tai toissijaisen näytön tai palautus-sivuston Office.

### <a name="what-we-did"></a>Olemme asentaminen

Näin, mikä on testin läpäissyt:

1. Luoda näennäiskoneiden VMM mallien avulla.

1. Aloittaminen näennäiskoneiden ja tallentaa perusaikataulun suorituskyvyn mittarit 12 tuntia.

1. Luotu paveikslėlis perus- ja hyödyntämistä VMM-palvelimiin.

1. Määritetty cloud Azure palauttaminen, mukaan lukien lähde- ja paveikslėlis määritys-kohdan suojaus.

1. Käytössä näennäiskoneiden suojaus ja että ne voivat valmis ensimmäinen replikointi.

1. Odotti muutaman tunnin järjestelmän vakiinnuttamiseksi.

1. Siepata suorituskyvyn mittarit yli 12 tuntia, joilla varmistetaan, että kaikki näennäiskoneiden olleet odotettu replikoinnin tilaan näiden 12 tuntia.

1. Mittaa delta perusaikataulun suorituskyvyn mittarit ja replikoinnin suorituskyvyn mittarit välillä.

## <a name="test-deployment-results"></a>Käyttöönoton testitulokset

### <a name="primary-server-performance"></a>Ensisijaisen palvelimen suorituskykyyn

- Hyper-V replikan seuraa asynkronisesti muutokset ensisijaisen palvelimen kanssa tallennustilan vähimmäiskoko katseltavan lokitiedostoon.

- Hyper-V replikan käyttämällä itse säilyvät välimuistissa Pienennä IOPS yleisrasite seurantaa varten. Tallennetaan kirjoittaa VHDX muistiin ja tyhjentää niitä lokitiedostoon ennen aika, joka lokin lähetetään palautus-sivustoon. DVD-levyllä tyhjennys tapahtuu myös, jos merkinnät osumien ennalta enimmäismäärä.

- Alla olevassa kaaviossa näkyy muuttumattoman tilan IOPS katseltavan replikoinnin. Näemme, että yleisrasite replikoinnin vuoksi IOPS on noin 5 %, jossa on aivan vähän.

![Ensisijainen tulokset](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Hyper-V replikan käyttämällä muistin ensisijainen palvelimessa levyn suorituskyvyn parantamiseksi. Seuraavassa kaaviossa esitetyllä muistia yleisrasite ensisijainen klusterin jokaisessa palvelimessa on rajan. Yleiskustannus näkyy muistin on käyttämän replikoinnin verrattuna asennetun muistin kokonaismäärän Hyper-V-palvelimeen.

![Ensisijainen tulokset](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V replikassa on pienin suorittimen katseltavan. Kuten kaavio, replikoinnin katseltavan on 2-3 % solualue.

![Ensisijainen tulokset](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Toissijainen (palautus) palvelimen suorituskykyyn

Hyper-V replikan käyttää pienen muistin palautus-palvelimessa optimoi tallennustilan toimintoja. Kaavio on yhteenveto muistinkäytön palautus-palvelimeen. Yleiskustannus näkyy muistin on käyttämän replikoinnin verrattuna yhteensä asennetun muistin Hyper-V-palvelimeen.

![Toissijainen tulokset](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Summa i/o-toimintoa palautus-sivustossa on ensisijainen sivuston toimintoja kirjoittaminen funktio. Oletetaan, että Tarkista yhteensä i/o toimintoja, i/o-toimintojen summa verrattuna palautus-sivustossa ja kirjoita ensisijainen sivuston toimintoja. Kaavioiden osoittavat, että summa IOPS palautus-sivustossa on

- 1,5 kertaa Kirjoita IOPS – ensisijainen.

- Noin 37 prosenttia yhteensä IOPS ensisijainen sivustossa.

![Toissijainen tulokset](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Toissijainen tulokset](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>Vaikutus replikoinnin verkon käyttö

Keskiarvoon 275 Mt sekunnissa verkon kaistanleveyden käytettiin aiemmin luotuun kaistanleveyden 5 gt sekunnissa vastaan perus- ja palautus-solmujen (ja pakkaus on käytössä).

![Tulokset verkon käyttö](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Replikoinnin virtuaalikoneen suorituskykyyn vaikutus

Tärkeää huomiota on vaikutus replikoinnin tuotannon työmääriä näennäiskoneiden käytössä. Jos ensisijainen sivuston riittävästi valmisteltu replikoinnin, valitse työtaakkaa ei kannata olla mitään vaikutusta. Hyper-V replikan lightweight seuranta järjestelmä varmistaa, että käynnissä näennäiskoneiden työmääriä ei vaikuttaa muuttumattoman replikoinnin aikana. Tämä on esitetty seuraavassa kaaviot.

Tämä kaavio näyttää IOPS suorittaa näennäiskoneiden eri toiminnoista ennen ja jälkeen replikointi on käytössä. Huomaat, että ei ole eroa kahden.

![Replikan vaikutus tuloksiin](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Seuraavassa kuvassa näkyy siirtonopeuden näennäiskoneiden eri toiminnoista ennen ja jälkeen replikointi on käytössä. Huomaat, että replikointia ei ole merkittäviä vaikutusta.

![Tulokset replikan tehosteet](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Tekemistä

Selvästi tulokset osoittavat, että Azure sivuston palautus on Hyper-V replikan Skaalaa hyvin pienin yleisrasite suuri klusterin kanssa.  Azure palauttaminen on yksinkertainen käyttöönoton, replikoinnin, hallinnasta ja valvonta. Hyper-V replika sisältää tarvittavat infrastruktuurin onnistuneen replikoinnin skaalauksen. Parhaan mahdollisen käyttöönoton suunnittelun Suosittelemme lataat [Hyper-V replikan kapasiteetin suunnittelu](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Ympäristön yksityiskohdat

### <a name="primary-site"></a>Ensisijainen

- Ensisijainen sivustossa on klusterin, joka sisältää viisi Hyper-V-palvelimessa on 470 näennäiskoneiden.

- Näennäiskoneiden suoritat eri toiminnoista, ja kaikki Azure palauttaminen suojaus on käytössä.

- Tallennustilan klusterin solmun tarjoaa iSCSI SAN. Mallin Hitachi HUS130.

- Klusterin kunkin palvelimen on neljä verkon korttien (NIC) yhden Gbps.

- Kaksi verkkokortit muodostanut yhteyden iSCSI yksityisverkon ja kaksi muodostanut yhteyden ulkoisen yrityksen verkossa. Ulkoiset verkostot on varattu vain klusterin yhteyksiä varten.

![Ensisijainen laitteistovaatimukset](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Palvelin|RAM-MUISTIA|Malli|Suoritin|Suorittimien määrä|NIC: SSÄ|Ohjelmiston|
|---|---|---|---|---|---|---|
|Hyper-V palvelimiin: <br />ESTLAB HOST11<br />ESTLAB HOST12<br />ESTLAB HOST13<br />ESTLAB HOST14<br />ESTLAB HOST25|128ESTLAB HOST25 on 256|Dell™ PowerEdge™ R820|Intel Xeon(R) suorittimen E5-4620 0 @ 2.20GHz|4|Voin Gbps x 4|Windows Server palvelinkeskuksen 2012 R2 (x64) + Hyper-V rooli|
|VMM Server|2|||2|1 Gbps|Windows Server-tietokannan 2012 R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Toissijainen (palautus)-sivustossa

- Toissijainen sivustossa on 6-solmu automaattisesti klusterin.

- Tallennustilan klusterin solmun tarjoaa iSCSI SAN. Mallin Hitachi HUS130.

![Ensisijainen laitteiston määritykset](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Palvelin|RAM-MUISTIA|Malli|Suoritin|Suorittimien määrä|NIC: SSÄ|Ohjelmiston|
|---|---|---|---|---|---|---|
|Hyper-V palvelimiin: <br />ESTLAB HOST07<br />ESTLAB HOST08<br />ESTLAB HOST09<br />ESTLAB HOST10|96|Dell™ PowerEdge™ R720|Intel Xeon(R) suorittimen E5-2630 0 @ 2.30GHz|2|Voin Gbps x 4|Windows Server palvelinkeskuksen 2012 R2 (x64) + Hyper-V rooli|
|ESTLAB HOST17|128|Dell™ PowerEdge™ R820|Intel Xeon(R) suorittimen E5-4620 0 @ 2.20GHz|4||Windows Server palvelinkeskuksen 2012 R2 (x64) + Hyper-V rooli|
|ESTLAB HOST24|256|Dell™ PowerEdge™ R820|Intel Xeon(R) suorittimen E5-4620 0 @ 2.20GHz|2||Windows Server palvelinkeskuksen 2012 R2 (x64) + Hyper-V rooli|
|VMM Server|2|||2|1 Gbps|Windows Server-tietokannan 2012 R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>Palvelimen toiminnoista

- Testaus on kerätty käytettyjä yrityksen asiakkaan tilanteissa toiminnoista.

- Käytämme [IOMeter](http://www.iometer.org) koottu simuloinnissa taulukkoon työmäärää ominaisuus kanssa.

- Kaikki IOMeter-profiileista on määritetty kirjoittaa satunnainen tavua, jotta saadaan aikaan huonoimman kirjoittaa kuvioiden määrittäminen toiminnoista.

|Kuormituksen|I/o koko (kt)|Accessin %|% Luku|Avoin i|I/o-kaava|
|---|---|---|---|---|---|
|Tiedostopalvelimeen|48163264|60 % 20 %5 %5 % 10 %|80 % 80 % 80 % 80 % 80 %|88888|Kaikki 100 %, satunnainen|
|SQL Server (osa 1) SQL Server (aseman 2)|864|100 %: n 100 %|70 %0 %|88|100 %: n random100 % peräkkäisiä|
|Exchange|32|100 %|67 prosentin|8|100 %: n satunnainen|
|Työasema/VDI|464|66 % 34 prosentin|70 % 95 %|11|Satunnainen sekä 100 %|
|Tiedoston verkkopalvelin|4864|33 34 prosentin 33 %|95 % 95 % 95 %|888|Kaikki 75 % satunnaisia|

### <a name="virtual-machine-configuration"></a>Virtuaalikoneen määritys

- 470 näennäiskoneiden ensisijainen klusterin.

- Kaikki näennäiskoneiden VHDX avulla.

- Näennäiskoneiden on koottu taulukkoon toiminnoista. Kaikki on luotu VMM malleja.

|Kuormituksen|# VMs|Pienin RAM-Muistia (gt)|Suurin RAM-Muistia (gt)|AM koon loogisen levyn (gt)|Suurin IOPS|
|---|---|---|---|---|---|
|SQL Server|51|1|4|167|10|
|Exchange-palvelimeen|71|1|4|552|10|
|Tiedostopalvelimeen|50|1|2|552|22|
|VDI|149|.5|1|80|6|
|Verkkopalvelin|149|.5|1|80|6|
|KOKONAISSUMMA|470|||96.83 TT|4108|

### <a name="azure-site-recovery-settings"></a>Azure sivuston palautusasetukset

- Azure sivuston palautus on määritetty paikalliset paikallisen suojaus

- VMM palvelimeen on neljä paveikslėlis määritetty, joka sisältää Hyper-V-klusterin palvelinten ja niiden näennäiskoneiden.

|Ensisijainen VMM pilveen|Suojatun näennäiskoneiden pilveen|Replikoinnin korkojakso|Palautuksen pisteet|
|---|---|---|---|
|PrimaryCloudRpo15m|142|15 minuutin|Ei mitään|
|PrimaryCloudRpo30s|47|30 ja osat|Ei mitään|
|PrimaryCloudRpo30sArp1|47|30 ja osat|1|
|PrimaryCloudRpo5m|235|5 minuuttia|Ei mitään|

### <a name="performance-metrics"></a>Suorituskyvyn mittarit

Taulukossa on kuvattu suorituskyvyn mittarit ja laskureita, jotka olivat mitattuna käyttöönoton.

|Arvo|Laskuri|
|---|---|
|SUORITIN|\Processor(_Total)\% suoritinaika|
|Käytettävissä oleva muisti|\Memory\Available Mtavua|
|IOPS|\PhysicalDisk (_Yhteensä) \Disk siirrot/s|
|AM (IOPS) toiminnot/s|\Hyper-V virtual tallennusväline (<VHD>) \Read valvontatoimintoja/s|
|AM kirjoittaminen (IOPS) valvontatoimintoja/s|\Hyper-V virtual tallennusväline (<VHD>) \Write toiminnot/S|
|Lue siirtonopeuden AM|\Hyper-V virtual tallennusväline (<VHD>) \Read tavua/s|
|AM kirjoittaminen siirtonopeuden|\Hyper-V virtual tallennusväline (<VHD>) \Write tavua/s|


## <a name="next-steps"></a>Seuraavat vaiheet

- [Määritä suojaus kaksi paikallisen VMM sivustosta toiseen](site-recovery-vmm-to-vmm.md)
