<properties
    pageTitle="Suorita Cassandra Linux Azure | Microsoft Azure"
    description="Suorittaminen Cassandra klusterin Linux Azuren näennäiskoneiden Node.js-sovelluksesta"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Cassandra käytön Linux Azure- ja Node.js avaaminen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>Yleiskatsaus
Microsoft Azure on Avaa cloud-ympäristössä, joka suoritetaan sekä Microsoft hyvin kuin muiden kuin Microsoft-ohjelmiston mukaan lukien käyttöjärjestelmät, sovelluksen palvelimissa, tekstiviesti middleware sekä SQL- ja NoSQL tietokannan sekä kaupallisten ja Avaa lähde-malleista. Rakentaminen julkisen paveikslėlis, mukaan lukien Azure joustavat services edellyttää varovainen suunnittelu ja tarkoituksellisen arkkitehtuuri molemmat sovellukset-palvelinten sekä tallennustilan kerrokset. Cassandra on jaettu tallennustilan arkkitehtuuri auttaa luonnollisesti muodostaminen erittäin käytettävissä järjestelmät, jotka ovat vikasietoisia klusterin virheet. Cassandra on cloud asteikko NoSQL tietokannan Apache ohjelmiston Foundation osoitteessa cassandra.apache.org; Cassandra on kirjoitettu Java ja näin ollen suorittaa molempien Windows sekä Linux-ympäristöissä.

Tässä artikkelissa keskitytään on Cassandra käyttöönoton näyttäminen Ubuntu kuin yksittäisten ja useiden tietojen center-klusterin hyödyntäminen Microsoft Azuren näennäiskoneiden ja Virtual verkot. Klusterin käyttöönottoa varten optimoitu tuotannon toiminnoista on tässä artikkelissa vaatii monen levy solmun kokoonpanossa, tarvittavat soi topologian rakenne ja tietomallien tukemaan tarvittavat replikoinnin, tietojen yhdenmukaisuuden, liikenteen ja suuren käytettävyyden vaatimukset.

Tässä artikkelissa on ratkaisevan lähestymistapa näyttämään mitä liittyy Cassandra-klusterin luominen verrattuna Docker, kokki tai joka voidaan infrastruktuurin käyttöönoton huomattavasti helpottavaa Puppet.  

## <a name="the-deployment-models"></a>Käyttöönotto-mallit
Microsoft Azure verkko sallii erillään yksityinen klustereiden, jonka access voidaan rajoittaa saavuttamiseksi tietoa hakuindeksointi voidaan suojata verkkosuojauksen käyttöönottoa.  Tässä artikkelissa on tietoja näkyy Cassandra käyttöönoton ratkaisevan tasolla, emme ole keskitytään yhdenmukaisuuden taso ja siirtonopeuden optimaalisen tallennustilan rakenne. Seuraavassa on luettelo verkko Microsoftin hypoteettista klusterin vaatimukset:

- Ulkoisten järjestelmien ei voi käyttää Cassandra tietokannan tai sen ulkopuolella Azure
- Cassandra klusterin pitää olla aikataulusta kuormituksen thrift tietoliikenteen
- Kaksi ryhmää kunkin data Centerissä parannettu klusterin käytettävyyden solmujen Cassandra käyttöönotto
- Lukitse klusterin niin, sovelluksen palvelinklusterin on pääsy tietokantaan suoraan vain
- Muut kuin SSH päätepisteitä Julkinen verkko
- Kukin Cassandra solmu on kiinteä sisäinen IP-osoite

Cassandra voidaan ottaa käyttöön vain yhden Azure alueen tai useita alueita, eri aikajaksoille laatu työmäärää perusteella. Monille käyttöönottomalli voidaan hyödyntää tukemaan loppukäyttäjät lähemmäs tietyn geography saman Cassandra infrastruktuuri kautta. Cassandra's valmiin solmu replikoinnin tulevat tarkkaan usean perustyylin synkronoinnin kirjoittaa useita tietojen keskikohdan mukaan peräisin ja esittää sovellusten tietojen perusteella. Riskien vähentäminen laajempi Azure palvelukatkoksia, myös apua monille käyttöönotto. Cassandra's tunable yhdenmukaisuutta ja replikoinnin topologian avulla kokouksen RPO monessa sovelluksia.

### <a name="single-region-deployment"></a>Yhden alueen käyttöönotto
Yhden alueen käytön aloittaminen ja sadonkorjuun learnings monille mallin luominen. Azure virtual verkko käytetään luomaan erillään aliverkosta niin, että edellä mainituista verkon suojausvaatimukset on täytettävä.  Yhden alueen käyttöönoton luominen kuvatut toimet käyttää Ubuntu 14.04 l. ja Cassandra 2.08; kuitenkin prosessi voi helposti annettava Linux-muunnoksia. Seuraavassa on joitakin yhden alueen käyttöönoton systeeminen ominaisuudet.  

**Suuren käytettävyyden:** Kuvassa 1 Cassandra solmujen otetaan käyttöön käytettävyys kahdet, niin, että solmut leviävät useita vika toimialueita suuren välillä. Käytettävyys kukin ehtojoukko kohdassa VMs on nyt yhdistetty 2 vika toimialueet.  Microsoft Azure käyttää vika toimialueen hallintaan suunnittelematon alaspäin kellonajan (kuten laitteiston ja ohjelmiston virhettä), kun päivityksen toimialueen (kuten isäntä- tai Vieras OS korjataan/päivitykset, sovelluksen päivitykset) käsite käsite käytetään hallinnassa, ajoitettua aikaa alaspäin. Lisätietoja vian rooli on [palauttaminen ja suuren käytettävyyden Azure-sovellusten](http://msdn.microsoft.com/library/dn251004.aspx) ja Päivitä toimialueiden saavuttamisessa suuren käytettävyyden.

![Yhden alueen käyttöönotto](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Kuva 1: Yhden alueen käyttöönotto

Huomaa, että tämä kirjoittaminen milloin Azure ei anna ryhmälle tietyn vika; toimialueen VMs eksplisiittinen yhdistäminen Näin ollen silloinkin kuvassa 1 käyttöönoton mallissa on tilastollinen todennäköistä, että kaikki näennäiskoneiden voi yhdistää kaksi vika toimialuetta neljä sijaan.

**Kuormituksen tasaaminen Thrift liikenne:** Asiakkaan thrift verkkopalvelin kirjastoja muodostaa sisäinen kuormituksen klusterissa. Tämä edellyttää sisäinen kuormituksen lisätään "tiedot"-osoitteiden prosessi (katso kuva 1) isännöinnin Cassandra klusterin pilvipalvelussa kontekstissa. Kun sisäinen kuormituksen on määritetty, kukin solmu edellyttää kuormitus tasataan päätepisteen kuormitus tasataan joukon aiemmin määritettyä kuormituksen tasauspalvelun nimellä huomautuksin lisätään. Saat lisätietoja [Azure sisäinen kuormituksen tasaamisen ](../load-balancer/load-balancer-internal-overview.md).

**Klusterin siemenet:** On tärkeää Valitse useimmat erittäin käytettävissä solmut siemenet, kuten uusien solmujen kommunikoi lähde solmujen tuttuihin klusterin topologian kanssa. Yhden solmun käytettävyys kunkin joukosta on määritetty lähde solmujen välttää virheen yhden pisteen.

**Replikoinnin kerroin ja yhdenmukaisuuden taso:** Cassandra's valmiiden suuren käytettävyyden ja tietojen kestävyyttä liittyy replikoinnin kerroin (RF - kunkin rivin tallennetun klusterin kopioiden lukumäärä) ja yhdenmukaisuuden taso (replikoita on ennen tulos palaaminen soittajan luku/kirjoitettu luku). Replikoinnin kerroin määritetään KEYSPACE (muistuttaa relaatiotietokannasta) luonnin aikana olisi yhdenmukaisuuden taso on määritetty aikana varmenteiden CRUD-kysely. Yhdenmukaisuuden lisätietoja [yhdenmukaisuuden määrittäminen](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) ja quorum laskenta laskentakaava Cassandra ohjeissa.

Cassandra tukee kahdentyyppisiä tietojen eheys mallien – yhdenmukaisuutta ja potentiaalisen yhdenmukaisuuden; Replikoinnin kerroin ja yhdenmukaisuuden taso yhdessä määräytyy Jos tiedot ovat yhdenmukaisia heti, kun kirjoitustoiminto on valmis, tai se on myöhemmin yhdenmukaisia. Esimerkiksi määrittäminen QUORUM kuin yhdenmukaisuuden taso aina varmistaa yhdenmukaisuuden kaikille tasoille tietojen kelpoisuus, määrä on kirjoitetaan saavuttamiseksi tarvittaessa alla QUORUM (esimerkiksi yksi) johtaa tietojen yhdenmukaisuuden myöhemmin.

8-solmu-klusterin yllä replikoinnin kerroin 3 ja QUORUM (2 solmujen on luku tai kirjoitettu yhdenmukaisuuden) luku-/ kirjoitusoikeudet yhdenmukaisuuden tasoa, voit uutta käyttöä varten enintään 1 solmu replikoinnin ryhmittäin ennen huomaamatta virheen sovelluksen Käynnistä teoreettinen menettämisen. Tämä oletetaan, että kaikki avaimen välilyöntejä on hyvin saapuva kirjoitus pyynnöt.  Käytämme käyttöön klusterin parametrit ovat seuraavat:

Yhden alueen Cassandra palvelinklusterin määritykset:

| Klusterin parametri | Arvo | Huomautuksia |
| ----------------- | ----- | ------- |
| (N) solmujen määrän | 8   | Klusterin solmut kokonaismäärä |
| Replikoinnin kerroin (RF) | 3 | Määrä on tietyn rivin |
| Yhdenmukaisuuden taso (Kirjoita) | QUORUM[(RF/2) +1) = 2] kaavan tulos pyöristetään alaspäin | Kirjoittaa enintään 2 replikoiden, ennen kuin vastaus lähetetään soittajan; 3 replikan kirjoitetaan myöhemmin yhtenäisellä tavalla. |
| Yhdenmukaisuuden taso (luku) | QUORUM [(RF/2) 1 = 2] kaavan tulos pyöristetään alaspäin | Lukee 2 replikoiden ennen lähettämistä vastauksen kutsuja. |
| Replikoinnin määrittäminen | NetworkTopologyStrategy tarkasteleminen [Tietojen replikoinnin](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) Cassandra ohjeissa | Ymmärtää käyttöönoton topologiasta ja sijoittaa replikoiden solmujen niin, että kaikki replikat ei sinut ohjataan saman Teline |
| Snitch | GossipingPropertyFileSnitch Katso [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra ohjeissa: | NetworkTopologyStrategy käyttää snitch käsite ymmärtää topologian. GossipingPropertyFileSnitch antaa parempia ohjausobjektin yhdistäminen kunkin solmun tietokeskuksen ja Teline. Klusterin käyttää juorut leviäminen nämä tiedot. Tämä on helpompaa suhteessa PropertyFileSnitch dynaaminen IP-asetus |


**Cassandra klusterin azure Huomioitavaa:** Microsoft Azuren näennäiskoneiden ominaisuus käyttää levyn pysyvyys; Azure-Blob-säiliö Azure-tallennustilan tallentaa 3 replikoiden kunkin levyn hyvin varmistamiseksi. Tämä tarkoittaa sitä, tiedot lisätään Cassandra taulukon kullakin rivillä on jo tallennettu 3 replikoita ja näin ollen tietojen yhdenmukaisuuden on jo tehdyt huolellisesti vaikka replikoinnin kerroin (RF) on 1. Replikoinnin kerroin on 1 tärkeimmät ongelma on, että sovellus ilmenee käyttökatkot, vaikka yksittäinen Cassandra solmu epäonnistuu. Jos solmu ei ole tunnistettu Azure kangasta valvoja ongelmat (esimerkiksi laitteiston ja ohjelmiston järjestelmävirheiden), se valmistelu uusi solmu entisen käyttämällä samaa tallennustilan asemat. Uusi solmu korvaa vanhan valmistelu voi kestää muutaman minuutin kuluttua.  Lisää vastaavasti suunnitellun ylläpidon toimintoihin, kuten Vieras OS muutokset, Cassandra päivittää ja sovelluksen muutoksia Azure kangasta ohjain suorittaa juoksevan päivitykset solmujen klusterin.  Liikkuva päivitykset myös saattaa kestää muutaman solmujen alaspäin kerrallaan ja näin ollen klusterin kokea lyhyt käyttökatkot muutaman osioiden. Kuitenkin tiedot eivät menetetään valmiin Azuren tallennustilaan redundancy vuoksi.  

Azure, joka ei edellytä suuren käytettävyyden (kuten ympärille 99,9, jossa on sama kuin 8.76 h/vuosi, katso lisätietoja [Suuren käytettävyyden](http://en.wikipedia.org/wiki/High_availability) ) käyttöön Systemsin yksinkertaisimmillaan toimimaan RF = 1 ja tason yhdenmukaisuuden = jokin.  Sovellusten suuren käytettävyyden vaatimusten RF = 3 ja yhdenmukaisuuden taso = QUORUM sallii yhden solmujen replikat jokin alaspäin aika. RF = 1-perinteinen ominaisuuksissa (esimerkiksi paikalliset) ei voi käyttää ongelmien, kuten levyn virheet johtuvat tietojen menetyksen vuoksi.   

## <a name="multi-region-deployment"></a>Monille käyttöönotto
Cassandra käyttäjän tiedot-center aware replikoinnin ja yhdenmukaisuuden mallin yllä olevien ohjeiden helpottaa monille käyttöönoton ruudun ilman mitään ulkoisen sillä edellyttää ulos. Tämä on aivan eri perinteinen relaatiotietokannoista, joissa asennus tietokannan kopiointiin usean pääkohteen kirjoituksia, voi olla varsin monimutkainen. Usean alueen määrittäminen Cassandra apua myös seuraavilla käytön skenaariot:

**Lähialue perusteella käyttöönoton:** Usean vuokraajan sovelluksia, Poista linkitys vuokraajan käyttäjät-,-alue on hyötyneet monille klusterin pienen viiveitä mukaan. Esimerkiksi learning hallinta oppilaitosten järjestelmien voit ottaa käyttöön Yhdysvaltojen Itä ja Länsi USA alueilla tukemaan vastaaviin virtuaalikampusten, eri aikajaksoille klusterin tapahtumien sekä analytics. Tietoja voi olla paikallisesti yhdenmukaisia aika lukuja ja kirjoituksia ja voit olevan tekstin yhdenmukaisia koko molemmat alueet. Muita esimerkkejä, kuten media jakauman, e-commerce ja mitään ja kaikki, joka on tiivistetty geo käyttäjän perus on hyvä Käyttötapaus käyttöönoton tämän mallin.

**Suuren käytettävyyden:** Redundancy on avainasemassa saavuttamisessa suuren käytettävyyden ohjelmistojen ja laitteistoon. Lisätietoja rakennuksen luotettava Cloud järjestelmien Microsoft Azure. Microsoft Azure vain luotettavasti saavuttaa tosi redundancy on ottamalla monille klusterin. Sovellusten voidaan ottaa käyttöön aktiivinen aktiivinen tai Aktiivinen-passiivinen-tilassa ja jokin alueiden ei ole käytettävissä, jos Azure liikenteen hallinta voit ohjata liikenne aktiivisen alueen.  Yhden alueen, käyttöönoton ja jos käytettävissä on 99,9, kahden alueen käyttöönotto voi saavuttamiseksi saatavuus on 99.9999 lasketaan kaavalla: (1-(1-0.999) *(1 0.999))*100); Katso lisätietoja edellä paperin.

**Palauttaminen:** Monille Cassandra klusterin voit Jos oikein suunniteltu kestävät kriittinen data Centerissä katkokset. Jos jonkin alueen ei ole käytettävissä, käyttöön muiden alueiden sovelluksen käynnistää tarjota käyttäjille. Kaikki muut liiketoiminnan jatkuvuuden käyttöotot, kuten sovelluksen on oltava varmatoimisempia asynkroninen myyntijakso tietojen tuloksena kadota tietoja. Kuitenkin Cassandra tekee palautus on paljon swifter kuin perinteisen tietokannan palauttaminen prosessit aika. Kuva 2 näyttää kunkin alueen tyypillinen monille käyttöönoton mallin kahdeksan solmujen kanssa. Molemmat alueet ovat peilikuva kuvia toisistaan samaan sijoitettava; käytännön rakenteen määräytyvät sen mukaan, kuormituksen tyyppi (kuten tapahtuma tai analytical), RPO, RTO, tietojen yhdenmukaisuuden ja käytettävyyden vaatimuksia.

![Usean alueen käyttöönotto](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Kuva 2: Monille Cassandra käyttöönotto

### <a name="network-integration"></a>Verkko-integrointi
Kummallakin alueella sijaitseva verkkojen käyttöön näennäiskoneiden tietojoukkoa yhteydessä toisiinsa käyttämällä VPN-tunnelin. VPN-tunnelin yhdistää kaksi ohjelmiston yhdyskäytävien valmistelun yhteydessä verkkoon käyttöönoton aikana. Molemmat alueet on samalla kannalta "WWW" ja "tiedot" aliverkosta, verkko-arkkitehtuuri Azure verkko avulla luotu niin monta aliverkosta tarpeen mukaan ja käyttää käyttöoikeusluettelot tarpeen mukaan verkkosuojaus. Klusterin topologian suunnitellessasi muun data Centerissä viestintä viive ja verkon liikenne ei tarvitse pitää taloudellisia vaikutuksia.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Tietojen yhdenmukaisuuden usean tietokeskuksen käyttöönottoa varten
Jaettu ominaisuuksissa tarvitse huomioon klusterin topologian vaikutus siirtonopeuden ja suuren käytettävyyden. RF ja yhdenmukaisuuden taso on valittava siten, että vuokaavio ei ole riippuvainen tietojen järjestelmänvalvojakeskuksiin käytettävyyttä.
Järjestelmän on hyvin yhdenmukaisuuden LOCAL_QUORUM yhdenmukaisuuden tason (varten ja lukujen kirjoituksia) varmistaa, että paikallinen lukee ja kirjoittaa on täytetty paikallisen solmut-, kun tiedot on replikoida asynkronisesti remote data keskikohdan mukaan.  Taulukon 2 on yhteenveto kuvatut myöhemmin kirjoitus monille klusterin tiedot.

**Kahden alueen Cassandra klusterimääritys**


| Klusterin parametri | Arvo | Huomautuksia |
| ----------------- | ----- | ------- |
| (N) solmujen määrän | 8 + 8 | Klusterin solmut kokonaismäärä |
| Replikoinnin kerroin (RF) | 3 | Määrä on tietyn rivin |
| Yhdenmukaisuuden taso (Kirjoita) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] kaavan tulos pyöristetään alaspäin | 2 solmujen on kirjoitettu ensimmäisen tietokeskuksen synkronoidusti; muut tarvittavat quorum 2 solmut kirjoitetaan asynkronisesti 2. tietokeskuksen. |
| Yhdenmukaisuuden taso (luku) | LOCAL_QUORUM ((RF/2) + 1) = 2 kaavan tulos pyöristetään alaspäin | Lue pyynnöt täyttyvät vain yhden alueen; 2 solmujen luetaan, ennen kuin vastaus lähetetään takaisin asiakkaalle. |
| Replikoinnin määrittäminen | NetworkTopologyStrategy tarkasteleminen [Tietojen replikoinnin](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) Cassandra ohjeissa | Ymmärtää käyttöönoton topologiasta ja sijoittaa replikoiden solmujen niin, että kaikki replikat ei sinut ohjataan saman Teline |
| Snitch | GossipingPropertyFileSnitch Katso [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra ohjeissa: | NetworkTopologyStrategy käyttää snitch käsite ymmärtää topologian. GossipingPropertyFileSnitch antaa parempia ohjausobjektin yhdistäminen kunkin solmun tietokeskuksen ja Teline. Klusterin käyttää juorut leviäminen nämä tiedot. Tämä on helpompaa suhteessa PropertyFileSnitch dynaaminen IP-asetus |


##<a name="the-software-configuration"></a>OHJELMISTON MÄÄRITYKSET
Käyttöönoton aikana voi käyttää ohjelmistoa seuraavia versioita:

<table>
<tr><th>Ohjelmiston</th><th>Lähde</th><th>Versio</th></tr>
<tr><td>JRE </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu  </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 L.</td></tr>
</table>

Koska lataaminen JRE vaatii manuaalisen hyväksymisen Oracle-käyttöoikeuden, yksinkertaistaa käyttöönotto-Lataa kaikki tarvittavat ohjelmistot myöhemmin ladataan kyselyjä Ubuntu-suunnittelumallin kuva on luovatko kuin muodostavien klusterin käyttöönottoon työpöydälle.

Lataa yllä ohjelmisto kansioon tunnetun lataaminen (kuten Windows %TEMP%/downloads tai ~/Downloads useimmat Linux jaot-tai Mac) paikalliseen tietokoneeseen.

### <a name="create-ubuntu-vm"></a>LUO UBUNTU AM
Tässä vaiheessa prosessin olemme Luo Ubuntu kuva valmiiksi edeltävät ohjelmistolla niin, että kuva voidaan käyttää useita Cassandra solmujen valmisteluun.  
####<a name="step-1-generate-ssh-key-pair"></a>Vaihe 1: Luo SSH avaimen pari
Azure on X509 julkisella avaimella, joka on PEM tai DER encoded valmistelu aikaan. Luo julkinen/yksityinen avaimen pari, miten voit käyttää SSH kanssa Linux Azure-osoitteessa annettujen ohjeiden mukaisesti. Jos aiot käyttää putty.exe SSH-asiakkaaksi joko Windows-tai Linux-on koodattu PEM muuntaa RSA yksityinen avain PPK muodossa puttygen.exe; ohjeet tähän löytyvät yllä web-sivua.

####<a name="step-2-create-ubuntu-template-vm"></a>Vaihe 2: Mallin Ubuntu AM luominen
Mallin AM luomiseen Kirjaudu Azure perinteinen portaalin ja käyttää seuraavassa järjestyksessä: Valitse uusi, suorittaminen, VIRTUAALIKONEEN, mistä VALIKOIMA, UBUNTU, Ubuntu Server 14.04 l. ja valitse sitten oikealle osoittavaa nuolta. Opetusohjelmaan, joka kuvaa Linux AM luomisesta on artikkelissa luominen virtuaalikoneen käynnissä Linux.

Anna seuraavat tiedot "virtuaalikoneen kokoonpanoa" näytössä #1:

<table>
<tr><th>KENTTÄNIMI              </td><td>       KENTTÄARVO               </td><td>         HUOMAUTUKSIA                </td><tr>
<tr><td>JULKAISUPÄIVÄ VERSIO    </td><td> Valitse drow päivämäärästä alaspäin</td><td></td><tr>
<tr><td>VIRTUAALIKONEEN NIMI    </td><td> Cass-malli                </td><td> Tämä on AM isäntänimi </td><tr>
<tr><td>TASO                     </td><td> VAKIO                        </td><td> Käytä oletusnimeä              </td><tr>
<tr><td>KOKOA                     </td><td> A1                              </td><td>Valitse AM IO; tarpeiden mukaan Käytä oletusnimeä tähän tarkoitukseen </td><tr>
<tr><td> UUDEN KÄYTTÄJÄNIMI           </td><td> localadmin                      </td><td> "Järjestelmänvalvoja" on varattu käyttäjänimen Ubuntu 12 xx ja sen jälkeen</td><tr>
<tr><td> TODENNUS      </td><td> Valitse valintaruutu                 </td><td>Tarkista, jos haluat suojata SSH-näppäimen kanssa </td><tr>
<tr><td> VARMENNE             </td><td> Julkisten avaimien varmenteen nimi </td><td> Käytä aiemmin luotu julkinen avain</td><tr>
<tr><td> Uusi salasana   </td><td> vahva salasana </td><td> </td><tr>
<tr><td> Vahvista salasana   </td><td> vahva salasana </td><td></td><tr>
</table>

Anna seuraavat tiedot "virtuaalikoneen kokoonpanoa" näytössä #2:

<table>
<tr><th>KENTTÄNIMI             </th><th> KENTTÄARVO                       </th><th> HUOMAUTUKSIA                                 </th></tr>
<tr><td> PILVIPALVELUSSA  </td><td> Luo uusi pilvipalvelussa    </td><td>Käytössäsi on säilö Laske-resurssit, kuten näennäiskoneiden</td></tr>
<tr><td> CLOUD PALVELUN DNS-NIMI </td><td>Ubuntu template.cloudapp.net   </td><td>Koneen agnostic kuormituksen tasauspalvelun nimi</td></tr>
<tr><td> ALUE/AFFINITEETTI RYHMÄN/VPN </td><td>    Länsi USA </td><td> Valitse alue, josta web-sovellusten käyttää Cassandra-klusterin</td></tr>
<tr><td>TALLENNUSTILAN TILIN </td><td>   Käytä oletusarvoa </td><td>Tallennustilan oletustilin tai valmiiksi luotuja tallennustilan tilin käyttäminen tietyllä alueella</td></tr>
<tr><td>KÄYTETTÄVYYS MÄÄRITTÄMINEN </td><td>  Ei mitään </td><td>  Jätä se tyhjäksi</td></tr>
<tr><td>PÄÄTEPISTEET   </td><td>Käytä oletusarvoa </td><td>  Oletusarvon SSH määritys </td></tr>
</table>

Oikealle osoittavaa nuolta, jätä #3-näytössä oletusarvot ja valitse Viimeistele prosessi valmistelu AM "Tarkista"-painiketta. Muutaman minuutin kuluttua AM nimi "ubuntu-mallin" pitäisi olla "käytössä-tila.

###<a name="install-the-necessary-software"></a>ASENNA TARVITTAVAT OHJELMISTOT
####<a name="step-1-upload-tarballs"></a>Vaihe 1: Lataa tarballs
Käyttämällä scp tai pscp, Kopioi aiemmin ladatun ohjelmiston ~/downloads-kansioon muodossa seuraava komento:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Toista yllä olevan komennon JRE samoin kuin Cassandra bittien.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>Vaihe 2: Valmisteleminen hakemistorakenne ja Pura arkistoon
Kirjaudu sisään AM ja luo kansiorakenne ja Pura ohjelmiston erittäin käyttäjänä alla bash-komentosarjan avulla:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Jos liität tämän komentosarjan vim-ikkunassa, varmista, että Poista rivinvaihto ('\r ") avulla seuraava komento:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Vaihe 3: Jne/profiilin muokkaaminen
Liitä loppuun seuraavasti:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Vaihe 4: Asenna JNA tuotannon Systemsin
Käytä komennot seuraavassa järjestyksessä: seuraava komento asennetaan jna 3.2.7.jar ja jna-ympäristö-3.2.7.jar /usr/share.java hakemistoon sudo piharakennus – Hae libjna-Javan asentaminen

Luo Symboliset linkit $CASS_HOME/lib-kansio, jonka Cassandra käynnistyskomentosarjan löytää nämä tölkki seuraavasti:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Vaihe 5: Määritä cassandra.yaml
Muokkaa cassandra.yaml, valitse kunkin AM vastaamaan mukaan kaikki [olemme säätää tehosteasetuksilla tämä todellinen valmistelun aikana] näennäiskoneiden tarvittavat määritykset:

<table>
<tr><th>Kenttänimi   </th><th> Arvo  </th><th> Huomautuksia </th></tr>
<tr><td>cluster_name </td><td>  "CustomerService"   </td><td> Käytä nimeä, joka kuvastaa käyttöönoton</td></tr>
<tr><td>listen_address  </td><td>[jätä se tyhjäksi]   </td><td> Poista "localhost" </td></tr>
<tr><td>rpc_addres   </td><td>[jätä se tyhjäksi]  </td><td> Poista "localhost" </td></tr>
<tr><td>siemenet   </td><td>"10.1.2.4 10.1.2.6, 10.1.2.8" </td><td>Kaikki, jotka on määritetty siemenet IP-osoitteiden luettelo.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Tätä käytetään johtamisprosessi tietokeskuksen ja AM Teline NetworkTopologyStrateg mukaan</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Vaihe 6: Siepata AM-kuva
Kirjaudu sisään virtuaalikoneen hostname (hk-cas-template.cloudapp.net) ja aiemmin luotu SSH yksityinen avain. Katso miten voit käyttää SSH Linux-Azure lisätietoja siitä, miten voit kirjautua käyttämällä komentoa ssh tai putty.exe kanssa.

Suorita toiminnot kannattaa tallentaa kuvan seuraavassa järjestyksessä:
#####<a name="1-deprovision"></a>1. deprovision
Komennolla "sudo waagent – deprovision + käyttäjän" Poista virtuaalikoneen esiintymän tietyt tiedot. Artikkelissa [sieppaaminen Linux-Virtual Machine](virtual-machines-linux-classic-capture-image.md) , Käytä mallina lisätietoja kuva sieppaaminen.

#####<a name="2-shutdown-the-vm"></a>2: Sammuta AM
Varmista, että virtuaalikoneen näkyy korostettuna ja sitten alas-komentopalkki Sammuta linkin.

#####<a name="3-capture-the-image"></a>3: siepata kuva
Varmista, että virtuaalikoneen näkyy korostettuna ja sitten alas-komentopalkki SIEPPAUS linkin. Anna KUVAN nimen (esimerkiksi hk-cas-2-08-ub-14-04-2014071) seuraavassa näytössä, tarvittaessa KUVAN kuvaus ja valitse "" valintamerkki sieppaaminen loppuun.

Tämä voi kestää joitakin sekunteja ja kuvan pitäisi olla käytettävissä, kuvavalikoiman Omat kuvat-osassa. Tietolähteen AM ole automaattisesti delated kuvan kirjataan onnistuneesti.

##<a name="single-region-deployment-process"></a>Yhden alueen käyttöönotto
**Vaihe 1: Luo virtuaalisia verkon** Kirjaudu Azure perinteinen portaalin ja luoda Virtual Network määritteet Näytä taulukko. Saat yksityiskohtaiset ohjeet prosessin [Azure perinteinen portaalissa Cloud-Only Virtual verkon määrittäminen](../virtual-network/virtual-networks-create-vnet-classic-portal.md) .      

<table>
<tr><th>AM määritteen nimi</th><th>Arvo</th><th>Huomautuksia</th></tr>
<tr><td>Nimi</td><td>vnet-cass-Länsi-yhteytta</td><td></td></tr>
<tr><td>Alue</td><td>Länsi USA</td><td></td></tr>
<tr><td>DNS-palvelimet </td><td>Ei mitään</td><td>Ohita tämä kuin emme Käytä DNS-palvelin</td></tr>
<tr><td>Pisteen sivuston VPN-verkon määrittäminen</td><td>Ei mitään</td><td> Ohita tämä</td></tr>
<tr><td>Määritä sivusto sivusto VPN</td><td>Nnone</td><td> Ohita tämä</td></tr>
<tr><td>Osoitetilaa varten</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>Ensimmäinen IP</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Lisää seuraavat aliverkosta:

<table>
<tr><th>Nimi</th><th>Ensimmäinen IP</th><th>CIDR</th><th>Huomautuksia</th></tr>
<tr><td>WWW</td><td>10.1.1.0</td><td>/ 24 (251)</td><td>WWW-klusterin aliverkon</td></tr>
<tr><td>tietoja</td><td>10.1.2.0</td><td>/ 24 (251)</td><td>Tietokannan solmujen aliverkon</td></tr>
</table>

Tietojen ja Web-aliverkosta voidaan suojata verkon käyttöoikeusryhmät soveltamisala, jossa on tämän artikkelin vaikutusalueen kautta.  

**Vaihe 2: valmistella näennäiskoneiden** Käytä aiemmin luotu kuva, emme luo seuraavat näennäiskoneiden cloud Serverissä "hk-c-svc-Länsi" ja sitoa niitä vastaaviin aliverkosta alla kuvatulla tavalla:

<table>
<tr><th>Tietokoneen nimi    </th><th>Aliverkon </th><th>IP-osoite </th><th>Käytettävyys määrittäminen</th><th>Toimialueen Ohjauskoneen/Teline</th><th>Määritä?</th></tr>
<tr><td>HK-c1-Länsi-yhteytta   </td><td>tietoja   </td><td>10.1.2.4   </td><td>HK-c-aset-1    </td><td>toimialueen ohjauskoneen = WESTUS Teline = rack1 </td><td>Kyllä</td></tr>
<tr><td>HK-c2-Länsi-yhteytta   </td><td>tietoja   </td><td>10.1.2.5   </td><td>HK-c-aset-1    </td><td>toimialueen ohjauskoneen = WESTUS Teline = rack1 </td><td>Ei </td></tr>
<tr><td>HK-c3-Länsi-yhteytta   </td><td>tietoja   </td><td>10.1.2.6   </td><td>HK-c-aset-1    </td><td>toimialueen ohjauskoneen = WESTUS Teline = rack2 </td><td>Kyllä</td></tr>
<tr><td>HK-c4-Länsi-yhteytta   </td><td>tietoja   </td><td>10.1.2.7   </td><td>HK-c-aset-1    </td><td>toimialueen ohjauskoneen = WESTUS Teline = rack2 </td><td>Ei </td></tr>
<tr><td>HK-c5-Länsi-yhteytta   </td><td>tietoja   </td><td>10.1.2.8   </td><td>HK-c-aset-2    </td><td>toimialueen ohjauskoneen = WESTUS Teline = rack3 </td><td>Kyllä</td></tr>
<tr><td>HK-c6-Länsi-yhteytta   </td><td>tietoja   </td><td>10.1.2.9   </td><td>HK-c-aset-2    </td><td>toimialueen ohjauskoneen = WESTUS Teline = rack3 </td><td>Ei </td></tr>
<tr><td>HK-c7-Länsi-yhteytta   </td><td>tietoja   </td><td>10.1.2.10  </td><td>HK-c-aset-2    </td><td>toimialueen ohjauskoneen = WESTUS Teline = rack4 </td><td>Kyllä</td></tr>
<tr><td>HK-c8-Länsi-yhteytta   </td><td>tietoja   </td><td>10.1.2.11  </td><td>HK-c-aset-2    </td><td>toimialueen ohjauskoneen = WESTUS Teline = rack4 </td><td>Ei </td></tr>
<tr><td>HK-w1-Länsi-yhteytta   </td><td>WWW    </td><td>10.1.1.4   </td><td>HK-w-aset-1    </td><td>                       </td><td>PUUTTUU</td></tr>
<tr><td>HK-w2-Länsi-yhteytta   </td><td>WWW    </td><td>10.1.1.5   </td><td>HK-w-aset-1    </td><td>                       </td><td>PUUTTUU</td></tr>
</table>

VMs yllä luettelon luominen edellyttää seuraavasti:

1.  Luo tyhjä pilvipalvelussa tietyllä alueella
2.  AM luominen aiemmin siepatun kuvan ja liitä se luotu aiemmin; virtual verkon Toista tämä kaikille VMs
3.  Lisää sisäinen kuormituksen pilvipalvelussa ja Liitä tiedosto "tiedot" aliverkon
4.  Lisää kunkin AM aiemmin luotu, Luo kuormitus tasataan päätepisteen thrift tietoliikenteen aiemmin luodun sisäinen kuormituksen yhteydessä kuormitus tasataan avulla

Yllä prosessi voidaan suorittaa Azure perinteinen portaalissa; Windows-tietokoneeseen (Käytä, jos sinulla ei ole Windows-tietokoneen pääsy Azure AM), käytä seuraavaa PowerShell-komentosarjaa avulla voit valmistella kaikki 8 VMs automaattisesti.

**Luettelo 1: PowerShell-komentosarjaa valmisteluun näennäiskoneiden**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Vaihe 3: Määrittämistä Cassandra kunkin AM**

Kirjaudu sisään AM ja tee seuraavat toimenpiteet:

* Muokkaa $CASS_HOME/conf/cassandra-rackdc.properties voit määrittää tietojen center ja Teline ominaisuudet:

       dc =EASTUS, rack =rack1

* Muokkaa cassandra.yaml määrittää lähde solmujen alla:

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Vaihe 4: Käynnistä VMs ja testaa klusterin**

Kirjaudu yhdeksi solmut (kuten hk-c1-Länsi-yhteytta) ja suorittamalla seuraavan komennon klusterin tilan tarkasteleminen:

       nodetool –h 10.1.2.4 –p 7199 status

Samanlainen kuin 8-solmu-klusterin näyttö tulee näkyviin:

<table>
<tr><th>Tila</th><th>Osoite  </th><th>Lataa   </th><th>Tunnusten </th><th>Omistaa </th><th>Host ()-Isäntätunnus  </th><th>Teline</th></tr>
<tr><th>POISTA  </td><td>10.1.2.4   </td><td>87.81 KT   </td><td>256    </td><td>38.0 %  </td><td>GUID-tunnus (poistaa)</td><td>rack1</td></tr>
<tr><th>POISTA  </td><td>10.1.2.5   </td><td>41.08 KT   </td><td>256    </td><td>68.9 %  </td><td>GUID-tunnus (poistaa)</td><td>rack1</td></tr>
<tr><th>POISTA  </td><td>10.1.2.6   </td><td>55.29 KT   </td><td>256    </td><td>68.8 %  </td><td>GUID-tunnus (poistaa)</td><td>rack2</td></tr>
<tr><th>POISTA  </td><td>10.1.2.7   </td><td>55.29 KT   </td><td>256    </td><td>68.8 %  </td><td>GUID-tunnus (poistaa)</td><td>rack2</td></tr>
<tr><th>POISTA  </td><td>10.1.2.8   </td><td>55.29 KT   </td><td>256    </td><td>68.8 %  </td><td>GUID-tunnus (poistaa)</td><td>rack3</td></tr>
<tr><th>POISTA  </td><td>10.1.2.9   </td><td>55.29 KT   </td><td>256    </td><td>68.8 %  </td><td>GUID-tunnus (poistaa)</td><td>rack3</td></tr>
<tr><th>POISTA  </td><td>10.1.2.10  </td><td>55.29 KT   </td><td>256    </td><td>68.8 %  </td><td>GUID-tunnus (poistaa)</td><td>rack4</td></tr>
<tr><th>POISTA  </td><td>10.1.2.11  </td><td>55.29 KT   </td><td>256    </td><td>68.8 %  </td><td>GUID-tunnus (poistaa)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Testaa yhden alueen klusterin
Seuraavien vaiheiden avulla voit testata klusterin:

1.    Käyttämällä Powershell-komentoa Get-AzureInternalLoadbalancer komentosovelmalla hankkia sisäinen kuormituksen IP-osoite (esimerkiksi  10.1.2.101). Komennon syntaksi on alla: Get-AzureLoadbalancer – palvelun nimi "hk-c-svc-Länsi-yhteytta" [näyttää tiedot sisäisten kuormituksen sen IP-osoite sekä]
2.  WWW-klusterin AM (kuten hk-w1-Länsi-yhteytta) on kirjauduttava käyttämällä painovärit, muste tai ssh
3.  Suorita $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4.  Seuraavat CQL komentojen avulla voit tarkistaa, toimiiko klusterin:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Näytä kuvattua pitäisi näkyä seuraavat:

<table>
  <tr><th> customer_id </th><th> Etunimi </th><th> Sukunimi </th></tr>
  <tr><td> 1 </td><td> Teemu </td><td> Doe </td></tr>
  <tr><td> 2 </td><td> Jane </td><td> Doe </td></tr>
</table>

Huomaa, että loit vaiheessa 4 keyspace käyttävä SimpleStrategy replication_factor 3. SimpleStrategy suositellaan yhtä center ominaisuuksissa olisi NetworkTopologyStrategy usean tietojen keskittää ominaisuuksissa. 3 replication_factor avulla solmu virheet poikkeama.

##<a id="tworegion"> </a>Monille käyttöönotto
Haluat hyödyntää yhden alueen käyttöönotto, joka on valmis ja sama toista asentaminen toisesta. Yksittäisten ja useiden alueen käyttöönoton avaimen ero on välisten alueen viestintään VPN tunnelin-määritys Olemme käynnistää verkkoasennuksen kanssa, valmistelu VMs ja Cassandra määrittäminen.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Vaihe 1: Luonti Virtual verkon 2. alue
Kirjaudu Azure perinteinen portaalin ja luoda Virtual Network määritteet Näytä taulukko. Saat yksityiskohtaiset ohjeet prosessin [Azure perinteinen portaalissa Cloud-Only Virtual verkon määrittäminen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) .      

<table>
<tr><th>Määritteen nimi    </th><th>Arvo    </th><th>Huomautuksia</th></tr>
<tr><td>Nimi    </td><td>vnet-cass-Itä-yhteytta</td><td></td></tr>
<tr><td>Alue  </td><td>Yhdysvaltojen Itä</td><td></td></tr>
<tr><td>DNS-palvelimet     </td><td></td><td>Ohita tämä kuin emme Käytä DNS-palvelin</td></tr>
<tr><td>Pisteen sivuston VPN-verkon määrittäminen</td><td></td><td>     Ohita tämä</td></tr>
<tr><td>Määritä sivusto sivusto VPN</td><td></td><td>      Ohita tämä</td></tr>
<tr><td>Osoitetilaa varten   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Ensimmäinen IP </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Lisää seuraavat aliverkosta:
<table>
<tr><th>Nimi    </th><th>Ensimmäinen IP    </th><th>CIDR   </th><th>Huomautuksia</th></tr>
<tr><td>WWW </td><td>10.2.1.0   </td><td>/ 24 (251)  </td><td>WWW-klusterin aliverkon</td></tr>
<tr><td>tietoja    </td><td>10.2.2.0   </td><td>/ 24 (251)  </td><td>Tietokannan solmujen aliverkon</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Vaihe 2: Luo paikalliset verkot
Lähiverkossa Azure virtual verkko on välityspalvelimen osoitetila, joka yhdistää etäsivuston, mukaan lukien yksityinen pilvestä tai toisen Azure alueen. Välityspalvelimen osoite tähän on sidottu remote yhdyskäytävän reititys verkon oikean verkko kohteisiin. Ohjeita VNET VNET yhteyden muodostamisesta on kohdassa [Configure VNet VNet yhteyttä](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) .

Luo kaksi paikallisen verkkojen kohti seuraavat asiat:

| Verkkonimi | VPN yhdyskäytäväosoite | Osoitetilaa varten | Huomautuksia |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-Map-to-East-US | 23.1.1.1  | 10.2.0.0/16   | Luomisen aikana lähiverkossa antaa paikkamerkin yhdyskäytäväosoite. Todellinen yhdyskäytäväosoite aihe yhdyskäytävän luomisen jälkeen. Varmista, että osoitetilaa vastaa vastaaviin remote VNET; Tässä tapauksessa VNET luotu Yhdysvaltojen Itä-alue. |
| HK-lnet-Map-to-West-US | 23.2.2.2  | 10.1.0.0/16   | Luomisen aikana lähiverkossa antaa paikkamerkin yhdyskäytäväosoite. Todellinen yhdyskäytäväosoite aihe yhdyskäytävän luomisen jälkeen. Varmista, että osoitetilaa vastaa vastaaviin remote VNET; Tässä tapauksessa VNET luotu Länsi US-alue. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Vaihe 3: Yhdistä "Paikallisen" verkon vastaaviin VNETs
Azure perinteinen-portaalista Valitse kunkin vnet, "Määritä", "Muodosta yhteys paikalliseen verkkoon kirjauduttaessa" Tarkista ja valitse paikallisen verkkojen kohti seuraavat asiat:


| VPN | Paikalliseen verkkoon kirjauduttaessa |
| --------------- | ------------- |
| HK-vnet-Länsi-yhteytta | HK-lnet-Map-to-East-US |
| HK-vnet-Itä-yhteytta | HK-lnet-Map-to-West-US |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Vaihe 4: Luominen yhdyskäytävien VNET1 ja VNET2
Valitse virtual verkkojen Raporttinäkymät-ikkunan, Luo yhdyskäytävä, joka käynnistää VPN-yhdyskäytävän valmistelu. Muutaman minuutin kuluttua kunkin virtual verkon Raporttinäkymät-ikkunan pitäisi näkyä todellinen yhdyskäytäväosoite.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Vaihe 5: Päivitä "Paikallisen" verkkojen vastaaviin "yhdyskäytävän"-osoitteet###
Muokkaa sekä paikallisen verkkojen paikkamerkin yhdyskäytävän IP-osoite korvaaminen vain valmistellun yhdyskäytävien reaali IP-osoite. Käytä seuraavia yhdistämistä:

<table>
<tr><th>Paikalliseen verkkoon kirjauduttaessa    </th><th>VPN-yhdyskäytävän</th></tr>
<tr><td>HK-lnet-Map-to-East-US </td><td>Yhdyskäytävän hk-vnet-Länsi-us.</td></tr>
<tr><td>HK-lnet-Map-to-West-US </td><td>Yhdyskäytävän hk-vnet-Itä-us.</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Vaihe 6: Päivittää jaetun avaimen
Käytä seuraavaa Powershell-komentosarjaa päivittämiseen kunkin VPN-yhdyskäytävän [käyttöä sekä yhdyskäytävien sake näppäintä] IP-näppäintä: Set-AzureVNetGatewayKey - VNetName hk-vnet-Itä-yhteytta - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-Länsi-yhteytta - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>Vaihe 7: VNET VNET-yhteyden luominen
Azure perinteinen työkaluportaaliin ja yhdyskäytävien-yhteyden virtual verkkojen "Raporttinäkymät-IKKUNAN"-valikon avulla. "Yhdistä" valikkovaihtoehdot käyttäminen alareunan työkalurivi. Muutaman minuutin kuluttua koontinäyttö pitäisi näyttää yhteystiedot graafisesti.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>Vaihe 8: Luo näennäiskoneiden alueen #2
Luo Ubuntu kuvan kuvatulla tavalla alueen #1 käyttöönoton noudattamalla samoja vaiheita tai Kopioi kuva Näennäiskiintolevytiedosto Azure tallennustilan tilille sijaitsevat alueen #2 ja kuvankäsittelyohjelmalla. Käytä tässä kuvassa ja näennäiskoneiden seuraavassa luettelossa Luo uusi pilvipalvelussa hk-c-svc-Itä-yhteytta:


| Tietokoneen nimi | Aliverkon | IP-osoite | Käytettävyys määrittäminen | Toimialueen Ohjauskoneen/Teline | Määritä? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| HK-c1-Itä-yhteytta | tietoja  | 10.2.2.4   | HK-c-aset-1      | toimialueen ohjauskoneen = EASTUS Teline = rack1 | Kyllä |
| HK-c2-Itä-yhteytta | tietoja  | 10.2.2.5   | HK-c-aset-1      | toimialueen ohjauskoneen = EASTUS Teline = rack1 | Ei  |
| HK-c3-Itä-yhteytta | tietoja  | 10.2.2.6   | HK-c-aset-1      | toimialueen ohjauskoneen = EASTUS Teline = rack2 | Kyllä |
| HK-c5-Itä-yhteytta | tietoja  | 10.2.2.8   | HK-c-aset-2      | toimialueen ohjauskoneen = EASTUS Teline = rack3 | Kyllä |
| HK-c6-Itä-yhteytta | tietoja  | 10.2.2.9   | HK-c-aset-2      | toimialueen ohjauskoneen = EASTUS Teline = rack3 | Ei  |
| HK-c7-Itä-yhteytta | tietoja  | 10.2.2.10  | HK-c-aset-2      | toimialueen ohjauskoneen = EASTUS Teline = rack4 | Kyllä |
| HK-c8 – Itä-yhteytta | tietoja  | 10.2.2.11  | HK-c-aset-2      | toimialueen ohjauskoneen = EASTUS Teline = rack4 | Ei  |
| HK-w1-Itä-yhteytta | WWW   | 10.2.1.4   | HK-w-aset-1      | PUUTTUU                    | PUUTTUU |
| HK-w2-Itä-yhteytta | WWW   | 10.2.1.5   | HK-w-aset-1      | PUUTTUU                    | PUUTTUU |


Samojen ohjeiden kuin alueen #1, mutta käytä 10.2.xxx.xxx osoitetilaa varten.

###<a name="step-9-configure-cassandra-on-each-vm"></a>Vaihe 9: Määrittämistä Cassandra kunkin AM
Kirjaudu sisään AM ja tee seuraavat toimenpiteet:

1. Muokkaa $CASS_HOME/conf/cassandra-rackdc.properties voit määrittää tietojen center ja Teline ominaisuudet muodossa: ohjauskoneen = EASTUS Teline = rack1
2. Muokkaa cassandra.yaml määrittäminen lähde solmujen: siemenet: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>Vaihe 10: Aloita Cassandra
Kirjaudu sisään kunkin AM ja aloita Cassandra taustalla suorittamalla seuraavan komennon: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>Testaa monille klusterin
Nyt mukaan Cassandra on otettu käyttöön 16 solmujen 8 solmujen Azure kummallakin alueella. Nämä solmut ovat samassa klusterissa klusterin kutsumanimi ja lähde-solmu-määritys. Klusterin etenee seuraavien vaiheiden avulla:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Vaihe 1: Hanki sisäinen kuormituksen tasauspalvelun IP sekä alueiden PowerShellin avulla
- Hae AzureInternalLoadbalancer - palvelun nimi "hk-c-svc-Länsi-yhteytta"
- Hae AzureInternalLoadbalancer - palvelun nimi "hk-c-svc-Itä-yhteytta"  

    Huomaa IP-osoitteet (kuten Länsi - 10.1.2.101, Itä - 10.2.2.101) näytetään.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Vaihe 2: Suorittaa seuraavat Länsi alueen kirjautumisessa hk-w1-Länsi-yhteytta jälkeen
1.    Suorita $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2.  Suorita seuraavat CQL-komennot:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Näytä kuvattua pitäisi näkyä seuraavat:

| customer_id | Etunimi | Sukunimi |
| ----------- | --------- | -------- |
| 1           | Teemu      | Doe      |
| 2           | Jane      | Doe      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Vaihe 3: Suorita Itä-alueella seuraavat kirjautumisessa hk-w1-Itä-yhteytta jälkeen:
1.    Suorita $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2.  Suorita seuraavat CQL-komennot:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Saman näyttö tulee näkyviin alueen Länsi tarkastelu:


| customer_id | Etunimi | Sukunimi   |
|------------ | --------- | ---------- |
| 1           | Teemu      | Doe        |
| 2           | Jane      | Doe        |


Suorittaa muutama Lisää Lisää ja tarkista, että ne Hae replikoida Länsi-yhteyttä klusterin osa.

## <a name="test-cassandra-cluster-from-nodejs"></a>Testaa Cassandra klusterin Node.js
Käytä jotakin, "WWW" crated Linux VMs taso aiemmin, emme suorittaa yksinkertaisen Node.js komentosarjan lukemaan aiemmin lisätyt tiedot

**Vaihe 1: Node.js ja Cassandra-asiakasohjelman asentaminen**

1. Asenna Node.js ja npm
2. Solmun package "cassandra-asiakasohjelman" asentaminen käyttämällä npm
3. Suorita seuraava komentosarja shell tulee näkyviin, joka näyttää haetut tiedot json-merkkijono:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Tekemistä
Microsoft Azure on joustava ympäristö, jonka avulla Microsoft sekä Avaa lähde-ohjelmiston toiminnan, kun tämän Harjoitus osoituksena. Erittäin käytettävissä Cassandra klustereiden voit ottaa käyttöön yhden tietokeskuksen kautta klusterisolmut levittäminen vika useita toimialueita. Cassandra klustereiden ottaa käyttöön myös tietojen todiste Systemsin useita maantieteellisesti kaukana Azure alueiden välillä. Azure ja Cassandra yhdessä ottaa käyttöön laajasti skaalattava, erittäin käytettävissä rakentaminen ja tietojen palautettavissa pilvipalveluihin tarpeen mukaan kuluvan päivän internet skaalata palvelut.  

##<a name="references"></a>Viittaukset##
- [http://cassandra.Apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
