<properties
   pageTitle="Microsoft cloud services ja verkon suojausta | Microsoft Azure"
   description="Lue joitakin ominaisuuksia käytettävissä Azure avulla voit luoda suojatun verkkoympäristöissä"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft cloud Services-palveluissa ja verkon suojaus

Microsoftin pilvipalveluihin toimittaa hyperscale palvelut ja infrastruktuuri, yrityksen arvosanojen valmiudet ja useita vaihtoehtoja hybrid yhteyksiä varten. Asiakkaat voivat valita käyttää näiden palvelujen Internetin kautta tai Azure ExpressRoute, jossa on yksityinen verkkoyhteyden kanssa. Microsoft Azure-ympäristön avulla voit laajentaa niiden infrastruktuurin pilvipalveluun ja luoda monitasoinen arkkitehtuureihin saumattomasti asiakkaat. Lisäksi kolmansien osapuolten ottaa parannettuihin tarjoamalla suojaustoimintoja ja virtual laitteiden. Tämä tekninen raportti esitetään yleiskatsaus suojaus ja arkkitehtuuri ongelmat, jotka asiakkaat huomioon, kun käyttämällä Microsoftin pilvipalveluihin käyttää ExpressRoute kautta. Se kattaa myös turvallisempi services luominen Azure virtual verkot.

## <a name="fast-start"></a>Nopea aloitus
Seuraava logiikan kaavion ohjata voit tietyn Esimerkki Azure ympäristössä käytettävissä monta suojauksen avulla. Etsi pikaopas, esimerkiksi parhaiten tapaus. Lisätietoja selitykset edelleen lukeminen paperin kautta.
![Suojausasetusten vuokaavio][0]

[Esimerkki 1: Muodosta ympäröivän verkon (tunnetaan myös nimellä DMZ, demilitarized vyöhyke ja tarkistettu varmenne aliverkon) voit suojata sovellusten verkon suojausryhmien (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Esimerkki 2: Luo ympäröivän verkon suojata sovellusten palomuuri ja NSGs.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Esimerkki 3: Muodosta ympäröivän verkon suojaavat verkkoa palomuuri, käyttäjän määrittämä reitin (UDR) ja NSG.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Esimerkki 4: Lisää hybrid yhteyden sivusto sivusto, virtuaalinen laitteen näennäisen yksityisverkon (VPN).](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Esimerkki 5: Lisää hybrid sivusto sivusto, Azure yhdyskäytävän VPN-yhteyttä.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Esimerkki 6: Lisää ExpressRoute hybrid-yhteyttä.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Esimerkkejä virtual verkoissa, suuri käytettävyys ja palvelun ketjutus välisten yhteyksien lisääminen lisätään tämän asiakirjan seuraavien kuukausien päälle.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft-yhteensopivuus ja infrastruktuurin suojaus
Microsoft on tehnyt tukevat yhteensopivuuden hankkeita yritysasiakkaille vaatii johtajuus toimen. Seuraavassa on joitakin yhteensopivuuden kaltaisilta Azure varten: ![Azure yhteensopivuuden merkkejä][1]

Lisätietoja on artikkelissa [Microsoft Valvontakeskus](https://azure.microsoft.com/support/trust-center/compliance/)yhteensopivuuden tiedot.

Microsoft on täydellinen lähestymistapa suojaaminen suorittamiseen hyperscale yleinen palvelut-pilvi-infrastruktuuria. Microsoft-pilvi-infrastruktuuria sisältää laitteiston, ohjelmisto, verkkojen ja järjestelmänvalvojan ja toimintojen henkilökunnan fyysinen palvelinkeskusten lisäksi.

![Azure suojaustoiminnot][2]

Tämän menetelmän on turvallisempi foundation asiakkaat voivat ottaa käyttöön niiden palveluiden Microsoft-pilvipalvelussa. Seuraava vaihe on tarkoitettu asiakkaille, suunnitella ja luoda suojausarkkitehtuuri suojaaminen palveluista.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Perinteinen suojauksen arkkitehtuureihin ja eteisverkot
Vaikka Microsoft toimittaja raskaasti suojaaminen pilvi-infrastruktuuria, asiakkaiden on myös suojata niiden pilvipalveluihin ja resurssiryhmät. Monikerroksisesta lähestymistapa suojaus on paras tapa. Ympäröivän verkon suojausvyöhykkeen suojaa sisäinen verkkoresursseja luotettu verkosta. Ympäröivän verkon viittaa reunoja tai verkko-osat, istut Internet- ja suojatun yrityksen IT-infrastruktuurin välillä.

Tyypillinen enterprise-verkoissa core infrastruktuuri on raskaasti tislausta perimeters kerroksia suojauslaitteet, jossa on. Kerroksen reunan koostuu laitteet ja käytännön käyttäminen pistettä. Laitteet voivat olla seuraavia: palomuurit, Distributed eston palvelun (WWW.microsoft.com-sivustoa kohtaan) estäminen, tunkeutumisen tunnistus tai suojauksen järjestelmien (tunnukset/IP-Osoitteet) ja VPN-laitteet. Käytäntöjen voi kestää palomuurin käytäntöjä, käyttöoikeusluettelot (käyttöoikeusluettelot) tai reititys. Verkon hyväksymällä suoraan Internetistä saapuvan liikenteen linnaa ensimmäinen rivi on menetelmien estä kalastelu ja haitalliset liikenne yhdistelmä samalla kun edelleen oikeutettu pyynnöt verkostoon. Tämä liikenne reitittää suoraan ympäröivän verkon resursseja. Resurssin voi sitten "jutella" resurssien tarkempaa verkossa kauttakulkuliikennettä harjoittavien seuraava rajan kelpoisuuden ensimmäisen. Uloimmat kerroksen kutsutaan ympäröivän verkon, koska tämä verkko-osa on määritetty yhteyden Internetiin, yleensä joitakin lomakkeen suojauksen molemmille puolille kanssa. Seuraavassa kuvassa on esimerkki yhden aliverkon ympäröivän verkon yritysverkkoa, jossa on kaksi suojauksen rajat.

![Ympäröivän verkon yrityksen verkossa][3]

On useita arkkitehtuureihin käyttää toteuttamaan ympäröivän verkon, yksinkertainen kuormituksen WWW-klusterin eteen-useita aliverkon ympäröivän verkon varustettuja vaihteleva kunkin reunan estää liikenteen ja suojata yritysverkon tarkempaa kerrokset-palvelussa. Miten ympäröivän verkon muodostanut riippuu organisaation ja sen yleinen riski poikkeama erityistarpeita.

Kun asiakkaat siirtyä niiden työmääriä julkisen paveikslėlis, on tärkeää tukemaan Azure vastannut yhteensopivuuteen ja suojausvaatimukset ympäröivän verkon arkkitehtuurista samanlaisia ominaisuuksia. Tämä asiakirja sisältää ohjeita, kuinka asiakkaat voit luoda suojatun Verkkoympäristössä Azure-tietokannassa. Ohjeessa on ympäröivän verkon, mutta myös täydellinen keskustelun on monia verkkosuojaus. Seuraavat kysymykset ilmoittaa keskustelun:

- Miten Azure ympäröivän verkon voit muodostettu?
- Mitkä ovat käytettävissä luonnissa ympäröivän verkon Azure ominaisuuksia?
- Miten taustatietokantaan työmääriä voidaan suojata?
- Miten Internet-tietoliikenne hallitaan työmääriä Azure-tietokannassa?
- Miten paikallisen verkoissa voidaan suojata Azure ominaisuuksissa?
- Kun alkuperäisen Azure suojaustoiminnot käytetään ja kolmannen osapuolen laitteiden tai palveluja?

Seuraavassa kaaviossa on esitetty eri käyttölupiin Azure tarjoaa asiakkaille. Nämä kerrokset ovat alkuperäisessä itse Azure alustalle ja asiakkaan määrittämät ominaisuudet:

![Azure suojausarkkitehtuuri][4]

Saapuvan Internetistä auttaa suojaamaan vastaan suurissa kalastelu vastaan Azure Azure WWW.microsoft.com-sivustoa kohtaan. Seuraava taso on asiakkaan määrittämät julkisen päätepisteitä, jotka määritetään liikenne voidaan kulkevat pilvipalvelussa sijaitsevaan virtual verkkoon. Alkuperäisen Azure virtual verkon eristystaso varmistaa valmis erillään muiden verkkojen ja että liikenne vain kulkee käyttäjän määritetty polkujen ja menetelmiä. Nämä polut ja menetelmät eivät seuraava kerros, johon NSGs, UDR ja verkon virtual laitteiden avulla voidaan luoda suojaus suojaaminen suojatun verkon sovelluksen-käyttöönotoissa rajat.

Seuraavassa osassa kerrotaan Azure virtual verkostojen. Virtuaalinen verkostojen asiakkaiden luodaan, ja ne ovat mitä niiden käyttöön työmääriä muodostanut yhteyden. Virtuaalinen verkkoja perustana olevan kaikki verkon suojaustoiminnot tarvitaan ympäröivän verkon suojaaminen asiakkaan ominaisuuksissa Azure-tietokannassa.

## <a name="overview-of-azure-virtual-networks"></a>Azure virtual verkkojen yleiskatsaus
Ennen kuin Internet-liikenne pääsevät Azure virtual verkkoihin, on kaksi käyttölupiin perusominaisuuksiin Azure ympäristö:

1.  **Www.microsoft.com-sivustoa kohtaan suojaus**: www.microsoft.com-sivustoa kohtaan suojaus on Azure fyysinen verkko, joka suojaa Azure platform itse suurissa Internet-pohjaisten viesteiltä. Nämä kalastelu avulla useita "robotti" solmujen yritettäessä ärsyttävä liikaa täyttää Internet-palveluun. Azure on tehokkaat WWW.microsoft.com-sivustoa kohtaan suojaus, valitse kaikki saapuvat Internet-yhteys silmukan. Kerroksen WWW.microsoft.com-sivustoa kohtaan suojaus ei ole käyttäjän määritettäviä määritteitä ja ei ole käytettävissä asiakkaalle. Suojaa Azure kuin suurissa viesteiltä ympäristö, mutta eivät suojaa yksittäisen asiakkaan sovelluksen suoraan. Lisää kerrokset toimintakykyyn voi määrittää asiakkaan lokalisoitu hyökkäystä vastaan. Jos asiakkaan A on hyökkäyksen kohteeksi suurissa WWW.microsoft.com-sivustoa kohtaan hyökkäyksen julkisen päätepisteen kanssa, Azure estää palvelun yhteydet. Asiakkaan A saattaa epäonnistua päälle toiseen virtual verkkoon tai palvelua liittyvistä ei palauta palvelun hyökkäyksen päätepisteen. On huomattava, vaikka asiakkaan A voi vaikuttaa kyseisen päätepisteen, ei muiden palvelujen ulkopuolella, päätepisteen vaikuttaa. Lisäksi muita asiakkaat ja palveluja näkee ei ole vaikutusta, hyökkäykseltä.
2.  **Palvelupäätepisteet**: päätepisteet Salli cloud services tai resurssien ryhmä on julkinen Internet-IP-osoitteista ja porteista tarjoamia. Päätepisteen käyttää verkko-osoitteen käännöksen (NAT) ja haluat reitittää liikenteen sisäinen osoite ja portin Azure virtual verkossa. Tämä on ensisijainen polku ulkoisen tietoliikenteen välittää virtual verkostoon. Palvelupäätepisteet ovat käyttäjä voi määrittää liikenne määritettäviä on ohitettu ja miten ja missä se muuntaa virtual verkossa.

Kun liikenne saavuttaa virtual verkossa, on useita ominaisuuksia, jotka tulevat toista kyselyjä. Azure virtual verkot ovat asiakkaat voivat liittää niiden työmääriä ja johon basic verkon käyttäjätason suojauksen sovelletaan Foundationin. Se on Azure yksityisverkon (VPN-kerros) asiakkaille, joilla seuraavat toiminnot ja ominaisuudet:
 
- **Liikenne eristystaso**: virtual verkko on liikenne eristystaso rajan Azure-ympäristössä. Yhden virtual verkoston näennäiskoneiden (VMs) ei voi muodostaa suoraan VMs eri virtual verkossa, vaikka molemmat virtual verkot luodaan saman asiakkaan. Tämä on kriittinen ominaisuus, joka takaa, että asiakkaan VMs ja communication pysyy yksityinen virtual verkossa.
- **Multitier topologian**: Virtual verkoissa asiakkaiden multitier topologian varaamalla aliverkosta ja nimeävät eri elementtien tai "tasoa" niiden työmääriä eri osoite välilyöntejä. Nämä looginen Ryhmittelyt ja topologioissa perustuvat työmäärää eri käyttöoikeuskäytäntö asiakkaiden ottaminen käyttöön ja hallita myös että tasojen välillä.
- **Paikallisen - yhteys**: asiakkaiden muodostaa virtual verkossa ja paikallisen sivustoissa tai virtual muiden verkkojen väliset yhteydet paikallisen-Azure-tietokannassa. Voit tehdä tämän asiakkaat voivat käyttää Azure VPN yhdyskäytävien tai kolmannen osapuolen verkon virtual laitteiden. Azure tukee sivusto sivusto (S2S) VPN-yhteydet käyttämällä vakio IP/IKE protokollat ja ExpressRoute yksityinen yhteys.
- **NSG** avulla asiakkaat voivat luoda sääntöjä (käyttöoikeusluettelot) rakeisuuden haluamasi tasolla: verkon liittymät, yksittäisiä VMs tai virtual aliverkosta. Asiakkaat, voit ohjata käytön salliminen tai estäminen välisen työmääriä virtual verkossa, järjestelmistä asiakkaan verkoissa paikallisen-yhteyden kautta tai suora Internet-yhteyden.
- **UDR** ja **IP-osoitteen välityksen** Salli asiakkaita eri tasojen virtual verkossa oleville välissä viestintä-polkujen määrittäminen. Asiakkaiden voit ottaa käyttöön palomuuri, tunnukset/IP-Osoitteet ja muut virtual laitteiden ja reitittää verkkoliikenteen – suojaus reunan käytännön käyttäminen, valvonnan ja tarkastus näiden suojaus-laitteiden.
- **Verkon virtual laitteiden** Azure Marketplacesta: suojaus-laitteita, kuten palomuurit, kuormituksen tasoitusmääritykset ja tunnukset/IP-Osoitteet ovat käytettävissä Azure Marketplacesta ja AM kuvavalikoima. Asiakkaat voivat ottaa käyttöön näiden laitteiden niiden virtual verkkojen ja erityisesti niiden suojauksen rajat (mukaan lukien ympäröivän verkon aliverkosta), viimeistele peruspalveluita suojatun Verkkoympäristössä.

Näitä ominaisuuksia ja toimintoja, joiden esimerkki siitä, miten rajat-verkoston arkkitehtuuri voitu muodostaa Azure-tietokannassa on seuraava:

![Ympäröivän verkon Azure virtual verkossa][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Edustan verkko-ominaisuuksien ja vaatimukset
Ympäröivän verkon on suunniteltu verkon vuorovaikutuksessa suoraan Internet-tietoliikenteen edusta. Saapuvien pakettien olisi flow – suojaus laitteita, kuten palomuurin, tunnukset ja IP-Osoitteet, ennen saapumista taustatietokantaan-palvelimiin. Internet sidottujen paketteja toiminnoista voi myös flow – suojaus laitteiden ympäröivän verkon, käytäntöjen, tarkastus ja valvonta varten, ennen kuin poistut verkon. Lisäksi ympäröivän verkon ylläpitää paikallisen-VPN yhdyskäytävien asiakkaan virtual verkkojen ja paikallisen verkkojen välillä.

### <a name="perimeter-network-characteristics"></a>Edustan verkko-ominaisuudet
Viittaaminen edellisessä kuvassa, jotkin hyvä ympäröivän verkon ominaisuudet ovat seuraavat:

- Internetiin yhteydessä oleva:
    - Itse ympäröivän verkko-osoitteiden on Internet-käyttöön, suoraan yhteydessä Internetiin.
    - Julkiseen IP-osoitteet, VIPs ja/tai Palvelupäätepisteet välittää Internet-liikenne edusta-verkko- ja laitteet.
    - Internet-liikenteen kulkee suojauksen laitteiden ennen muita resursseja edusta verkossa.
    - Jos lähtevien suojaus on käytössä, liikenne kulkee suojauksen laitteiden viimeisessä vaiheessa, ennen kuin kulkee Internet.
- Suojatun verkon:
    - Ei ole suora polku Internetistä core-infrastruktuuria.
    - Kanavien core-infrastruktuuria on käy läpi – suojaus-laitteita, kuten NSGs, palomuurit tai VPN-laitteet.
    - Muissa laitteissa on silta ei ole Internet- ja core-infrastruktuuria.
    - Internet-käyttöä ja suojatun verkon vastakkaisten reunojen ympäröivän verkon (esimerkiksi kaksi palomuurin kuvakkeet edellisessä kuvassa) suojaus-laitteisiin todella ehkä yksittäinen virtual laitteen eriteltyjen sääntöjä tai liittymät kunkin reunaa varten. (Eli yhteen laitteeseen, loogisesti erotettu käsittely molempien reunojen ympäröivän verkon lataus.)
- Muita yleisiä käytännöt ja rajoitukset:
    - Työmääriä ei tule tallentaa business tärkeitä tietoja.
    - Käyttöoikeudet ja ympäröivän verkon määritysten ja ominaisuuksissa rajoittuvat vain valtuutetut järjestelmänvalvojat.

### <a name="perimeter-network-requirements"></a>Ympäröivän Verkkovaatimukset
Näiden ominaisuuksien käyttöön VPN-vaatimuksista toteuttamisesta onnistuneen ympäröivän verkon noudattamalla seuraavia ohjeita:

- **Aliverkon arkkitehtuuri:** Määritä virtual verkon siten, että koko aliverkon on varattu ympäröivän verkon, samassa virtual verkossa muiden aliverkosta erotettava nimellä. Tällä varmistetaan, että liikenne ympäröivän verkon ja muut sisäinen tai yksityinen aliverkon tasoa muodostuvalle palomuuri tai tunnukset/IP-Osoitteet virtual laitteen aliverkon rajojen ja käyttäjän määrittämät tiet välillä.
- **NSG:** Itse ympäröivän verkko-osoitteiden on oltava auki, jotta viestintä yhteydessä Internetiin, mutta ei tarkoittaa asiakkaiden ohittaminen NSGs. Toimi Yleiset suojauskäytäntöjä Pienennä verkko-Näyttömalli käyttää on Internet-yhteyttä. Lukitse voi käyttää käyttöönotoissa tai tietyn sovelluksen protokollat ja portit, jotka ovat avoinna remote osoitealueet. Voi olla seuraavissa tilanteissa, joissa tämä ei voi aina. Esimerkiksi jos asiakkaita on ulkoinen sivusto Azure-tietokannassa, ympäröivän verkon olisi Salli web-julkiseen IP-osoitteita pyynnöt, mutta älä avaa web-sovelluksen portit: TCP:80 ja TCP:443.
- **Reititys-taulukon:** Itse ympäröivän verkko-osoitteiden pitäisi olla yhteydessä Internetiin suoraan, mutta älä salli suora viestintä ja sieltä pois takaisin end tai paikallisen verkkojen avaamatta tai laitteen.
- **Laitteen suojausasetukset:** Reitittää ja tutkimalla pakettien ympäröivän verkon ja muiden suojatun verkkojen, kuten palomuurin, tunnukset ja IP-Osoitteet suojaus-laitteiden välillä laitteet voi olla usean verkkoon. Ne voi olla erilliset NIC ympäröivän verkon ja taustatietokantaan aliverkosta. NIC-ympäröivän verkon viestiä suoraan ja Internetistä vastaavan NSGs ja ympäröivän verkon reititys-taulukossa. Yhteyden taustatietokantaan aliverkosta NIC Lisää rajoittanut NSGs ja vastaavan taustatietokantaan aliverkosta reititys taulukot.
- **Laitteen toimintoa:** Suojaus-laitteiden käyttöön ympäröivän verkon yleensä suorittaa seuraavia toimintoja:
    - Palomuurin: Pakotetut palomuurisäännöt tai access-ohjausobjektin käytännöt pyynnöt.
    - Uhkien tunnistaminen ja estäminen: tunnistaminen ja pienentävät haittaohjelmien kalastelu Internetistä.
    - Tarkistaminen ja kirjaamisen: ylläpito yksityiskohtaiset lokit tarkistaminen ja analysointia varten.
    - Käänteinen välityspalvelimen: uudelleenohjausta pyynnöt vastaavan taustatietokantaan-palvelimiin. Tämä liittyy yhdistäminen ja kääntäminen kohdeosoitteet edusta-laitteissa, yleensä palomuuria, taustatietokantaan server-osoitteisiin.
    - Välittää välityspalvelimen: tekemistä tarkistaminen Internet virtual verkoston käynnistetty viestintään ja antamisen NAT.
    - Reitittimen: Välittäminen saapuvan ja rajat aliverkon liikenteen virtual verkoston sisällä.
    - VPN-laite: visuaalisessa muodossa paikallisen-VPN IP-osoitteiden välillä asiakkaan paikallisen verkkojen ja Azure virtual verkkojen paikallisen-VPN-yhteys.
    - VPN-palvelin: hyväksymällä Azure virtual verkkojen muodostaa VPN-asiakkaille.

>[AZURE.TIP] Seuraavat kaksi ryhmää erottaa: henkilöiden oikeutta käyttää ympäröivän verkon suojauksen vaihde ja sovelluksen kehitystä, käyttöönottoa tai toiminnot järjestelmänvalvojia valtuutettuja henkilöitä. Pitäminen eri ryhmiin eriyttämisen avulla ja estää yhdelle henkilölle ei voi ohittaa sovellusten suojaus ja verkon turvaominaisuudet.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Kysymyksiä pyydetään luotaessa verkon rajat
Tässä osassa paitsi erikseen mainitun termi "verkkojen" viittaa yksityinen Azure virtual verkkoihin tilauksen järjestelmänvalvojan luomia. Termi ei viittaa pohjana fyysinen verkoissa Azure kuluessa.

Azure virtual verkkoja käytetään myös usein siirtämisestä perinteinen paikallisen verkot. On mahdollista toteuttamaan sivusto sivusto tai ExpressRoute hybrid verkko ratkaisuja ympäröivän verkon arkkitehtuureihin. Tämä on otettava huomioon muodostaminen verkon suojauksen rajat.

Seuraavat kolme kysymykset ovat kriittisiä vastaa, kun luot ympäröivän verkon ja useita suojauksen rajat verkosta.

#### <a name="1-how-many-boundaries-are-needed"></a>1) kuinka monta rajat tarvitaan?
Ensimmäinen päätöskohta on päättää, kuinka monta suojauksen rajat tarvitaan annetun tilanne:

- Yksittäisen reunan: yksi edusta ympäröivän verkossa virtual verkko ja Internet välillä.
- Kaksi rajat: yksi reunassa Internet ympäröivän verkon ja toinen ympäröivän verkko-osoitteiden ja Azure virtual verkkojen taustatietokantaan-aliverkosta välillä.
- Kolme rajat: yksi ympäröivän verkon Internet-puolella, yksi ympäröivän verkon ja taustatietokantaan aliverkosta välillä ja yksi taustatietokantaan aliverkosta ja paikallisen verkon välillä.
- N rajat: muuttujan numero. Sen mukaan, suojausvaatimukset on todella haluamasi määrän suojauksen rajat, joka voi suojata annetun verkossa.

Määrän ja tyypin, rajat tarvitaan vaihtelee yrityksen riskin poikkeama ja tietty skenaario, joka on otettu käyttöön. Tämä on usein tekemät organisaation usein esimerkiksi riskin ja vaatimustenmukaisuus ryhmän verkon ja ympäristö ryhmän ja -sovelluksen ryhmälle useita ryhmiä joint päätös. Suojaus, liittyvät tiedot ja käytössä technologies ihmisiin pitäisi olla että päätökseen riittävät asenteesta kunkin toteutuksen varmistamiseksi.

>[AZURE.TIP] Käytä rajat, jotka täyttävät tietyn tilanteen suojausvaatimukset pienimmän luvun. Lisää rajat vaikeaa ja toiminnot ja vianmääritys voi olla, sekä hallinta-katseltavan liittyvistä hallinta useita reunan käytäntöjä ajan kuluessa. Kuitenkin riittämätön rajat lisätä riskejä. On ehdottoman etsiminen saldo.

![Hybrid verkossa, jossa on kolme suojauksen rajat][6]

Edellä olevassa kuvassa näkyy kolme suojauksen reunan verkon ylätason näkymän. Rajat ovat ympäröivän verkon ja Internetin, Azure edusta- ja yksityiset aliverkosta, ja Azure taustatietokantaan aliverkon-paikallisen yrityksen verkkoon.

#### <a name="2-where-are-the-boundaries-located"></a>2) missä rajat sijaitsevat?
Kun rajat määrä on päätetty, mistä ne toteuttaa on seuraava päätöskohta. Liittyy yleensä kolmesta vaihtoehdosta:
- Internet-pohjaisten välittävät-palveluun (esimerkiksi pilvipohjainen Web sovelluksen palomuuri, joka ei ole tässä asiakirjassa kerrotaan)
- Azure alkuperäisen ominaisuuksia ja/tai verkon virtual laitteiden käyttäminen
- Fyysisten laitteiden käyttäminen paikalliseen verkkoon

Vaihtoehdot ovat puhtaasti Azure-verkoissa alkuperäisen Azure ominaisuuksia (esimerkiksi Azure kuormituksen tasoitusmääritykset) tai verkon virtual laitteita monipuolisia kumppanin ekosysteemissä, Azure (esimerkiksi Check Point palomuurit).

Jos rajan tarvita – Azure paikalliseen verkkoon, suojaus-laitteiden voi sijaita molemmille puolille yhteyden (tai molemmat puolet). Näin päätös on tehtävä sijainti suojauksen Ratas paikoilleen.

Edellisessä kuvassa Internet ympäröivän verkon eteen Edellinen-päähän rajat sisältyvät kokonaisuudessaan Azure ja täytyy olla alkuperäisen Azure ominaisuuksia tai verkon virtual laitteita. Suojaus-laitteisiin Azure (taustatietokantaan aliverkon) ja yrityksen verkkoon välissä olevaa reunaa voi olla joko Azure puolella tai paikallisen tilitietoja tai jopa yhdistelmän laitteiden molemmille puolille. Voi olla merkittäviä hyötyjä ja haittoja joko vaihtoehto, joka on otettava huomioon vakavasti.

Esimerkiksi käyttämällä aiemmin fyysinen turvallisuus vaihde paikalliseen verkkoon reunassa on se etu, että uusi vaihde ei tarvita. Se on vain kokoonpanon uudelleenmääritys. Haitta on kuitenkin, että kaikki liikenne on peräisin takaisin Azure voi tarkastella suojauksen vaihde paikalliseen verkkoon. Näin Azure Azure liikenne voi syntyä merkittäviä viive ja sovelluksen suorituskykyyn vaikuttavat ja käyttäjän ilmenee, jos se on pakotettu takaisin paikalliseen verkon suojauksen käytännön käyttäminen.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) kuinka rajat käyttöön?
Suojauksen kunkin reunan on todennäköisesti eri ominaisuuksien tarkistaminen (esimerkiksi tunnukset ja palomuurisäännöt ympäröivän verkon, mutta ainoastaan taustatietokannan aliverkon ja ympäröivän verkon välillä käyttöoikeusluettelot Internet reunassa). Mitkä laitteet käyttämään päättäminen määräytyy skenaarion ja suojaus-vaatimukset. Seuraavassa osassa esimerkkejä 1, 2 ja 3 käsitellään joitakin asetuksia, joita voi käyttää. Azure alkuperäisen verkko-ominaisuuksia ja Azure kumppanin ekosysteemissä käyttämiseen laitteiden tarkistaminen näyttää käytettävissä ratkaisemiseksi miltei mistä tahansa skenaarion myriad asetukset.

Toisen avaimen toteutuksen päätöskohta kerrotaan, miten voit muodostaa yhteyden paikalliseen verkkoon Azure. Kannattaa valita Azure virtual yhdyskäytävän tai verkon virtual laitteen? Nämä vaihtoehdot käsitellään tarkemmin seuraavassa osassa (esimerkkejä 4, 5 ja 6).

Lisäksi liikenne Azure virtual verkkojen välillä voivat olla tarpeen. Näissä tilanteissa lisätään myöhemmin.

Kun tiedät Edellinen kysymysten vastaukset, [Nopea aloitus](#fast-start) -osaan voi tunnistaa mitkä ovat tietyn tilanteeseen sopivimman.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Esimerkkejä: Building suojauksen rajat Azure virtual verkkojen kanssa
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Esimerkki 1: Muodosta ympäröivän verkon NSGs sovellusten suojaaminen
[Takaisin alkuun nopeasti](#fast-start) | [yksityiskohtaisessa luominen tässä esimerkissä ohjeet][Example1]

![Saapuvien ympäröivän verkon NSG kanssa][7]

#### <a name="environment-description"></a>Ympäristön kuvaus
Tässä esimerkissä on tilauksessa, joka sisältää seuraavat:

- Kaksi cloud services: "FrontEnd001" ja "BackEnd001"
- Virtuaalinen verkosta, "CorpNetwork" kahden aliverkon: "FrontEnd" ja "Taustajärjestelmä"
- Verkon käyttöoikeusryhmän, jota käytetään sekä aliverkosta
- Windows server, joka edustaa sovelluksen verkkopalvelin ("IIS01")
- Kaksi Windows-palvelimissa, jotka edustavat sovelluksen taustatietokantaan servers ("AppVM01", "AppVM02")
- Windows server, joka edustaa DNS server ("DNS01")

Komentosarjojen ja Azure Resurssienhallinta-malli on artikkelissa [Muodosta yksityiskohtaiset ohjeet][Example1].

#### <a name="nsg-description"></a>NSG kuvaus
Tässä esimerkissä NSG-ryhmän sisäisten ja ladata sitten kuusi säännöillä.

>[AZURE.TIP] Niinkään kannattaa luoda tietyn "Salli" sääntöjen ensin yleisen "Estä" sääntöjä perään. Annetun tärkeyden määrittää, mitkä säännöt arvioidaan ensin. Kun liikenne löytyy tietyn säännön koskee, arvioidaan edelleen sääntöjä ei ole. NSG säännöt käyttää jompaankumpaan suuntaan saapuvat tai Lähtevät (aliverkon näkökulmasta).

Määrittämisen avulla seuraavat säännöt rakennetaan saapuvan liikenteen:

1.  Sisäiseen DNS-liikenne (portti 53) on sallittua.
2.  Virtual koneen RDP liikenteen (portti 3389) Internetistä on sallittu.
3.  HTTP liikenne (portti 80) Internetistä WWW-palvelimeen (IIS01) on sallittua.
4.  Minkä tahansa liikenteen (kaikki portit)-IIS01 AppVM1 on sallittu.
5.  Liikenteen (kaikki portit) Internetistä koko virtual verkkoon (molemmat aliverkosta) on estetty.
6.  Minkä tahansa liikenteen (kaikki portit) edusta aliverkosta taustatietokantaan aliverkon on estetty.

Sääntöjen ja sitoa kunkin aliverkon, jos HTTP-pyyntö on ollut saapuva Internetistä WWW-palvelimeen, sekä sääntöjä 3 (Salli) ja 5 (estä) sovelletaan. Mutta sääntö 3 on suuri prioriteetti, se vain sovelletaan ja säännön 5 tule toista kyselyjä. Näin HTTP-pyyntö voivat verkkopalvelin. Jos sama liikenne yritti saavuttaa DNS01 palvelimeen, sääntö 5 (estä) on ensimmäinen sovelletaan ja liikenne eivät voi siirtää palvelimeen. Säännön 6 (estä) estää edusta aliverkon-puhelu taustatietokantaan aliverkon (lukuun ottamatta sallittu liikenteen säännöt 1 – 4). Tämä suojaa taustatietokantaan verkon siltä varalta, että luvaton hakujärjestelmän edusta WWW-sovellusta. Hän on rajoitetut käyttöoikeudet taustatietokantaan "suojattu" verkon (vain resursseja tarjoamia AppVM01 palvelimessa).

Ei oletusarvoisesti lähtevän säännön, joka sallii Internet-liikenteen ulos. Tässä esimerkissä on olet sallimisen liikenteen ja muokkaaminen ei ole mitään lähtevän liikenteen säännöt. Lukitse liikenne molempiin suuntiin, käyttäjän määrittämä reititys on pakollinen (Katso Esimerkki 3).

#### <a name="conclusion"></a>Tekemistä
Tämä on suhteellisen yksinkertainen ja selkeä tapa käyttöjarrujärjestelmän taustatietokantaan aliverkon saapuvan liikenteen. Lisätietoja on artikkelissa [Muodosta yksityiskohtaiset ohjeet][Example1]. Nämä ohjeet ovat seuraavat:

- Miten voit luoda tämän ympäröivän verkon PowerShell-komentosarjojen kanssa.
- Miten voit luoda tämän ympäröivän verkon Azure Resurssienhallinta-malli.
- Yksityiskohtainen kuvaus NSG jokaisen komennon.
- Yksityiskohtainen liikenne sähköpostiskenaarioiden määrittämisessä, miten liikenne sallittu tai estetty kerroksia.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Esimerkki 2: Luo ympäröivän verkon sovellusten palomuuri ja NSGs suojaaminen
[Takaisin alkuun nopeasti](#fast-start) | [yksityiskohtainen luominen tässä esimerkissä ohjeet][Example2]

![Saapuvien ympäröivän verkon NVA ja NSG][8]

#### <a name="environment-description"></a>Ympäristön kuvaus
Tässä esimerkissä on tilauksessa, joka sisältää seuraavat:

- Kaksi cloud services: "FrontEnd001" ja "BackEnd001"
- Virtuaalinen verkosta, "CorpNetwork" kahden aliverkon: "FrontEnd" ja "Taustajärjestelmä"
- Verkon käyttöoikeusryhmän, jota käytetään sekä aliverkosta
- Edustatietokannan aliverkon yhteydessä, tässä tapauksessa palomuuri, joka verkossa virtual laitteen
- Windows server, joka edustaa sovelluksen verkkopalvelin ("IIS01")
- Kaksi Windows-palvelimissa, jotka edustavat sovelluksen taustatietokantaan servers ("AppVM01", "AppVM02")
- Windows server, joka edustaa DNS server ("DNS01")

Komentosarjojen ja Azure Resurssienhallinta-malli on artikkelissa [Muodosta yksityiskohtaiset ohjeet][Example2].

#### <a name="nsg-description"></a>NSG kuvaus
Tässä esimerkissä NSG-ryhmän sisäisten ja ladata sitten kuusi säännöillä.

>[AZURE.TIP] Niinkään kannattaa luoda tietyn "Salli" sääntöjen ensin yleisen "Estä" sääntöjä perään. Annetun tärkeyden määrittää, mitkä säännöt arvioidaan ensin. Kun liikenne löytyy tietyn säännön koskee, arvioidaan edelleen sääntöjä ei ole. NSG säännöt käyttää jompaankumpaan suuntaan saapuvat tai Lähtevät (aliverkon näkökulmasta).

Määrittämisen avulla seuraavat säännöt rakennetaan saapuvan liikenteen:

1.  Sisäiseen DNS-liikenne (portti 53) on sallittua.
2.  Virtual koneen RDP liikenteen (portti 3389) Internetistä on sallittu.
3.  Verkon virtual laitteeseen (palomuuri) minkä tahansa Internet-liikenne (kaikki portit) on sallittua.
4.  Minkä tahansa liikenteen (kaikki portit)-IIS01 AppVM1 on sallittu.
5.  Liikenteen (kaikki portit) Internetistä koko virtual verkkoon (molemmat aliverkosta) on estetty.
6.  Minkä tahansa liikenteen (kaikki portit) edusta aliverkosta taustatietokantaan aliverkon on estetty.

Sääntöjen ja sitoa kunkin aliverkon, jos HTTP-pyyntö on ollut saapuva Internetistä palomuuri, sekä sääntöjä 3 (Salli) ja 5 (estä) sovelletaan. Mutta sääntö 3 on suuri prioriteetti, se vain sovelletaan ja säännön 5 tule toista kyselyjä. Näin HTTP-pyyntö voivat palomuurin. Jos sama liikenne on muodostamassa yhteyttä IIS01 palvelimeen, vaikka se ei edusta aliverkon, sääntö 5 (estä) sovelletaan, liikenne ei ole sallittua siirtää palvelimeen. Säännön 6 (estä) estää edusta aliverkon-puhelu taustatietokantaan aliverkon (lukuun ottamatta sallittu liikenteen säännöt 1 – 4). Tämä suojaa taustatietokantaan verkon siltä varalta, että luvaton hakujärjestelmän edusta WWW-sovellusta. Hän on rajoitetut käyttöoikeudet taustatietokantaan "suojattu" verkon (vain resursseja tarjoamia AppVM01 palvelimessa).

Ei oletusarvoisesti lähtevän säännön, joka sallii Internet-liikenteen ulos. Tässä esimerkissä on olet sallimisen liikenteen ja muokkaaminen ei ole mitään lähtevän liikenteen säännöt. Lukitse liikenne molempiin suuntiin, käyttäjän määrittämä reititys on pakollinen (Katso Esimerkki 3).

#### <a name="firewall-rule-description"></a>Palomuurin säännön kuvaus
Palomuurin lähettäminen edelleen säännöt luodaan. Koska tässä esimerkissä reitittää vain Internet-liikenne-sidottujen palomuurin ja sitten WWW-palvelimeen vain yksi edelleenlähetys verkko-osoitteiden muuntaminen (NAT) sääntö tarvita.

Edelleenlähetyssäännön hyväksyy mikä tahansa saapuvien lähde-osoite, käynnit palomuurin haettavaa HTTP (portti 80 ja 443 HTTPS). Se on lähetetty palomuurin paikallisen liittymän ulos ja ohjataan verkkopalvelin 10.0.1.5 IP-osoite.

#### <a name="conclusion"></a>Tekemistä
Tämä on melko helppoa tapaa, jolla suojaaminen sovelluksesi, jossa on palomuuri, ja eristää kyseinen taustatietokantaan aliverkon saapuvan liikenteen. Lisätietoja on artikkelissa [Muodosta yksityiskohtaiset ohjeet][Example2]. Nämä ohjeet ovat seuraavat:

- Miten voit luoda tämän ympäröivän verkon PowerShell-komentosarjojen kanssa.
- Miten voit luoda tämän esimerkin Azure Resurssienhallinta-malli.
- Yksityiskohtainen kuvaus NSG-komento ja palomuurin säännöt.
- Yksityiskohtainen liikenne sähköpostiskenaarioiden määrittämisessä, miten liikenne sallittu tai estetty kerroksia.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Esimerkki 3: Muodosta ympäröivän verkon suojaavat verkkoa palomuuri, UDR ja NSG
[Takaisin alkuun nopeasti](#fast-start) | [yksityiskohtainen luominen tässä esimerkissä ohjeet][Example3]

![Kaksisuuntaisen ympäröivän verkon NVA, NSG ja UDR][9]

#### <a name="environment-description"></a>Ympäristön kuvaus
Tässä esimerkissä on tilauksessa, joka sisältää seuraavat:

- Kolme cloud services: "SecSvc001", "FrontEnd001" ja "BackEnd001"
- Virtual verkosta, "CorpNetwork" kolme aliverkosta: "SecNet", "FrontEnd" ja "Taustajärjestelmä"
- SecNet aliverkon yhteydessä, tässä tapauksessa palomuuri, joka verkossa virtual laitteen
- Windows server, joka edustaa sovelluksen verkkopalvelin ("IIS01")
- Kaksi Windows-palvelimissa, jotka edustavat sovelluksen taustatietokantaan servers ("AppVM01", "AppVM02")
- Windows server, joka edustaa DNS server ("DNS01")

Komentosarjojen ja Azure Resurssienhallinta-malli on artikkelissa [Muodosta yksityiskohtaiset ohjeet][Example3].

#### <a name="udr-description"></a>UDR kuvaus
Oletusarvoisesti seuraavat järjestelmän tiet määritellään seuraavasti:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal on aina määritetty osoite prefix(es) tietyn verkon virtual verkoston (eli se muuttuu VPN virtual verkkoon, sen mukaan, miten kukin tietyn virtual verkko on määritetty). Jäljellä oleva järjestelmän tiet on staattinen ja sellaisena kuin taulukon Oletus.

Tässä esimerkissä reititys taulukot luodaan, yksi kullekin edusta- ja -aliverkosta. Jokaisessa taulukossa on ladattu staattinen tiet tietyn aliverkon varten. Tässä esimerkissä varten jokaisessa taulukossa on kolme tiet, ohjata kaikki liikenne (0.0.0.0/0) palomuurin läpi (seuraava = Virtual laitteen IP-osoite):

1. Paikallisen aliverkon liikenteen seuraava siirräntävälien ei ole määritetty sallimaan paikallisen aliverkon liikenteen ohittaa palomuurin.
2. Virtual verkkoliikenteen kanssa seuraavan siirräntävälien lasketaan palomuurin; Tämä ohittaa oletusarvo-sääntö, joka sallii paikallisen virtual verkkoliikenteen reitittäminen suoraan.
3. Kaikki jäljellä olevat liikenteen (0/0) seuraava siirräntävälien palomuurin lasketaan.

>[AZURE.TIP] Ottaa ei paikallisen aliverkon tapahtuma UDR katkaista paikallisen aliverkon tietoliikenne. 
> - Tämän esimerkin osoittamalla VNETLocal 10.0.1.0/24 on ehdottoman poistumista muussa tapauksessa paketin toisen paikallisen palvelimen (esimerkiksi) 10.0.1.25 tarkoitettu Web-palvelimen (10.0.1.4) epäonnistuu, kun ne lähetetään päälle NVA, joka lähettää sen aliverkon, ja aliverkon uudelleen lähettää sen NVA ja niin edelleen.
> - Reititys-silmukan on yleensä vähintään usean nic-laitteita, joka on suoraan yhteydessä kunkin aliverkon ne keskustelet, joka on usein perinteinen, paikallisia, laitteita. 

Reititys taulukot luodaan, kun ne on sidottu niiden aliverkosta. Edustatietokannan aliverkon reititys taulukon, ja sitoo aliverkon, kun haluat näyttää tältä:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] UDR voi käyttää nyt yhdyskäytävän aliverkon, joina ExpressRoute piiri on yhdistetty.
>
> Esimerkkejä siitä, miten voit ottaa käyttöön ympäröivän verkon ExpressRoute tai sivuston sivuston verkko on esitetty esimerkkejä 3 ja 4:


#### <a name="ip-forwarding-description"></a>IP-osoitteen välityksen kuvaus
IP-osoitteen välityksen on avustaja-ominaisuus, jonka UDR. Tämä on virtual laitteen, jonka avulla saat liikenne ei ole nimenomaisesti osoitettu laitteen ja määritä edelleenlähetys ultimate perillepääsy liikenne-asetus.

Esimerkiksi jos AppVM01 tietoliikenteen tekee pyyntö DNS01 palvelimeen, UDR reitittää tämä palomuuri. IP-osoitteen välityksen käytössä DNS01 kohde (10.0.2.4) liikenne on hyväksynyt laitteen (10.0.0.4) ja siirtää sitten ultimate perillepääsy (10.0.2.4). Ilman IP-osoitteen välityksen käytössä palomuuri liikenne ei voi hyväksyä laitteen vaikka reititys-taulukossa on palomuurin kuin seuraavan kohteen. Käyttämään virtual laitteen on ehdottoman tärkeää muista ottaa IP-osoitteen välityksen UDR yhdessä.

#### <a name="nsg-description"></a>NSG kuvaus
Tässä esimerkissä NSG ryhmän sisäisten ja ladata sitten yksittäisen säännön. Tämän ryhmän sitten on sidottu vain edusta- ja aliverkosta (ei SecNet). Käyttöliittymän seuraavaa sääntöä on käännetty:

- Liikenteen (kaikki portit) Internetistä koko virtual verkkoon (kaikki aliverkosta) on estetty.

Tässä esimerkissä käytetään NSGs, mutta sen tärkeimmät tarkoituksena on toissijainen tasona linnaa manuaalinen virheellisen määrityksen mukaan. Jos haluat estää kaiken saapuvan liikenteen Internetistä joko edusta- tai aliverkosta lähinnä. Liikenne olisi vain flow SecNet aliverkon palomuurin läpi (ja paina sitten tarvittaessa voin edusta- tai -aliverkosta). Plus-UDR sääntöjen paikassa, liikenteestä, joka tekemällä siitä edusta- tai -aliverkosta ohjautuu ulos palomuurin (tuotteen UDR). Palomuurin nähdä tämän kuin julkiseen työnkulku ja pudota lähtevän tietoliikenteen. Näin on kolme suojaaminen aliverkosta käyttölupiin:
- Avaa päätepisteitä FrontEnd001 ja BackEnd001 cloud services.
- NSGs estäminen Internet-liikenne.
- Palomuurin poistetaan julkiseen liikenne.

Yksi kiinnostavasta kohdasta koskeva NSG tässä esimerkissä on, että se sisältää vain yksi sääntö, joka estää Internet-liikenne koko virtual verkkoon, kuten suojauksen aliverkon. Kuitenkin NSG on vain sidottu edusta- ja -aliverkosta, koska sääntö ei ole käsitellään liikenteen saapuva suojauksen aliverkon. Tuloksena liikenne flow suojaus on käytettävissä aliverkon ulkopuolella.

#### <a name="firewall-rules"></a>Palomuurisäännöt
Palomuurin lähettäminen edelleen säännöt luodaan. Koska palomuuri estää tai kaikki saapuva, lähtevä edelleenlähetys ja sisäiset virtual verkkoliikenteen, tarvitaan monta palomuurisäännöt. Myös kaikki saapuva liikenne osumien suojaus-palvelun julkiseen IP-osoite (eri portit), palomuurin käsittelemien. Paras käytäntö on looginen kulkee ennen kuin määrität aliverkosta kaavio ja palomuurisäännöt, jotta Uudista myöhemmin. Seuraavassa kuvassa on looginen näkymän tässä esimerkissä palomuurisäännöt:
 
![Palomuurisäännöt loogisten näkymä][10]

>[AZURE.NOTE] Käyttää verkon Virtual laitteen mukaan hallinta-portit vaihtelevat. Tässä esimerkissä Barracuda NextGen palomuuri on viittaus, joka muotoilee portit 22 801 ja 807. Oppaasta laitteen toimittajan löydät tarkan portit, joita käytetään hallinta käytössä.

#### <a name="firewall-rules-description"></a>Palomuurin säännöt kuvaus
Edellä olevassa looginen kaaviossa suojauksen aliverkon ei ole näkyvissä. Tämä johtuu siitä palomuuri on vain kyseisen aliverkon resurssin ja tässä kaaviossa näkyy palomuurisäännöt ja kuinka ne loogisesti Salli tai estä liikenne kulkee ei todellinen reititetty polkua. Myös ulkoisten porttien valittu RDP-liikenne ylittävät vaihtelivat portit (8014 – 8026) ja valittiin hieman tasassa paikallinen IP-osoite on helpompi lukea kaksi oktettia (esimerkiksi paikalliset palvelimen osoite 10.0.1.4 on liitetty ulkoisen portin 8014). Mikä tahansa suurempi-ristiriitaiset portit, voi kuitenkin voi käyttää.

Tässä esimerkissä annettava seuraavia sääntöjä:

- Ulkoiset säännöt (saapuvan liikenteen):
  1.    Palomuurin hallinnan säännön: tämän sovelluksen uudelleenohjata säännön avulla liikenteen hallinta porttien verkon virtual laitteen.
  2.    RDP säännöt (kunkin Windows server): (yksi kullekin palvelimelle) neljä sääntöjen Salli hallinta kautta RDP yksittäisiin-palvelimiin. Tämä voi myös yhteen yhdeksi yksi sääntö mukaan käytössä verkon virtual laitteen ominaisuuksia.
  3.    Sovelluksen liikenteen säännöt: on kaksi sääntöjen edusta-WWW-tietoliikenteen ensimmäisen ja toisen taustatietokannan tietoliikenteen (esimerkiksi web-palvelin tietojen taso). Sääntöjen määrittäminen määräytyy (palvelinten sijoitetut) verkoston arkkitehtuuri ja liikennettä kulkee (suunnaksi, jota käytetään liikenne kulkee ja joka portit).
      - Ensimmäisen säännön avulla todellinen sovelluksen liikenne sovelluspalvelin saavuttamiseksi. Muut säännöt Salli suojaus ja hallinta-sovelluksen liikenteen säännöt ovat mitä Salli ulkoisten käyttäjien tai palveluja, jotka käyttävät sovellukset. Tässä esimerkissä porttiin 80 on yksittäisessä WWW-palvelimessa. Näin yhden palomuurin sovelluksen säännön ohjaa saapuvan liikenteen ulkoisessa IP web palvelinten sisäinen IP-osoitteeseen. Uudelleenohjatun liikenne istunnon kääntää kautta NAT sisäiselle palvelimelle.
      - Toisen säännön on mikä tahansa portti taustatietokantaan-sääntö, joka sallii verkkopalvelin voit jutella AppVM01 server (mutta ei AppVM02).
- Sisäinen säännöt (sisäiset virtual verkkoliikenteen)
  4.    Lähtevän säännön Internet: tämän säännön sallii tietoliikenteen verkon siirtää valitun verkot. Tämä sääntö on yleensä oletusarvon säännön jo käyttöön palomuuri, mutta ei käytössä-tilassa. Tässä esimerkissä tulisi olla käytössä tämän säännön.
  5.    DNS-säännön: tämän säännön avulla vain DNS (portti 53) liikenne siirtää DNS-palvelimeen. Tässä ympäristössä useimmat tietoliikenteen edustatietokanta taustatietokantaan on estetty. Tämän säännön avulla DNS erityisesti minkä tahansa paikallisen aliverkon.
  6.    Aliverkon aliverkon sääntöön: Tämä sääntö ei salli kaikki palvelimen taustatietokantaan aliverkon muodostaa yhteyden palvelimelle edusta aliverkon (mutta et päinvastoin).
- Käynnistystila (liikenteen sääntö, joka ei vastaa mitään edellisen):
  7.    Estä kaikki liikenne säännön: Tämä on aina oltava lopullinen säännön (kannalta prioriteetti) ja sellaisenaan Jos liikenne työnkulku ei vastaa mitään edeltävän säännöt poistetaan tämän säännön perusteella. Tämä on oletusarvo sääntö ja yleensä aktivoitu. Muutoksia ei yleensä tarvitaan.

>[AZURE.TIP] Toisen liikenne sääntö voidaan yksinkertaistaa tässä esimerkissä kaikki portit on sallittu. Todellinen tilanteessa tarkkaa portti- ja osoitealueet olisi käytettävä voit pienentää tämän säännön.

Kaikki edellisen säännöt luodaan, kun on tärkeää varmistaa liikenne sallittu tai estetty haluamallasi tavalla kunkin säännön ensisijaisuuden tarkistettava. Tässä esimerkissä säännöt ovat ensisijaisuusjärjestys.

#### <a name="conclusion"></a>Tekemistä
Tämä on monimutkaisia, mutta lisätietoja tapa suojaaminen ja eristää kyseinen kuin edellisessä kohdassa käytettyä verkkoon. (Esimerkiksi 2 suojaa sovelluksen ja esimerkissä 1 eristää vain aliverkosta). Tämän rakenteen liikenne molempiin suuntiin seuranta mahdollistaa ja suojaa eikä vain saapuvien sovelluspalvelin mutta pakottaa verkon suojauskäytäntö palvelimilta tässä verkossa. Myös käyttää laitteen mukaan koko liikenne tarkistaminen ja tilatieto onnistuu. Lisätietoja on artikkelissa [Muodosta yksityiskohtaiset ohjeet][Example3]. Nämä ohjeet ovat seuraavat:

- Miten voit luoda tämän esimerkin ympäröivän verkon PowerShell-komentosarjojen kanssa.
- Miten voit luoda tämän esimerkin Azure Resurssienhallinta-malli.
- Yksityiskohtaiset kuvaukset kunkin UDR NSG-komento ja palomuurisäännön.
- Yksityiskohtainen liikenne sähköpostiskenaarioiden määrittämisessä, miten liikenne sallittu tai estetty kerroksia.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Esimerkki 4: Lisää hybrid yhteyden sivusto sivusto, virtuaalinen laitteen näennäisen yksityisverkon (VPN)
[Takaisin alkuun nopeasti](#fast-start) | Tarkat muodosta ovat käytettävissä pian

![Ympäröivän verkon NVA yhdistetyn hybrid verkoston kanssa][11]

#### <a name="environment-description"></a>Ympäristön kuvaus
Hybrid verkko verkon: laitteen virtual (NVA) avulla voidaan lisätä mihin tahansa esimerkkejä 1, 2 tai 3 kuvattua rajat-verkkotyypit.

Edellisessä kuvassa osoitetulla Internetin välityksellä (sivuston sivuston) VPN-yhteyden avulla luodaan yhteys paikalliseen verkkoon Azure virtual verkkoon NVA kautta.

>[AZURE.NOTE] Jos käytät ExpressRoute Azure julkisen Peering-asetus on otettu käyttöön, luodaanko staattisen reitin. Tämä reitittää NVA VPN-IP-osoitteeseen yrityksen Internet- ja ei kautta ExpressRoute WAN. Pakollinen ExpressRoute Azure julkisen Peering-asetus NAT voivat katkaista VPN-istunnon.

Kun VPN-yhteys on määritetty, NVA keskuksena keskitetyn verkoissa ja aliverkosta. Palomuurin edelleenlähetyssääntöjä selvittää, mitkä liikenne kulkee sallittu, muunnetaan NAT kautta, ohjataan tai olevat kohteet poistetaan (myös varten liikenne kulkee paikalliseen verkkoon ja Azure välillä).

Liikenne kulkee tulee pitää huolellisesti, ne voidaan optimoida tai heikentynyt mukaan rakenne tätä mallia mukaan erityiset käyttötapaus.

Esimerkki 3 hallinta-ympäristön avulla ja sitten lisätä sivuston sivuston VPN hybrid on verkkoyhteys tuottaa seuraavat rakenne:

![Sivuston sivuston VPN yhdistetty ympäröivän verkon NVA kanssa][12]

Paikallisen reitittimen tai muita verkkolaitteeseen, joka on yhteensopiva VPN-yhteyttä NVA olisi VPN-asiakasohjelman. Fyysinen laite on vastuussa aloitetaan ja ylläpito oman NVA VPN-yhteyttä.

Loogisesti, NVA, verkon näyttää neljä erillistä "Suojausvyöhykkeet" sääntöjen parhaillaan liikenne välillä vyöhykkeiden ensisijainen johtaja NVA:

![Looginen verkko NVA näkökulmasta][13]

#### <a name="conclusion"></a>Tekemistä
Sivuston sivuston VPN hybrid verkkoyhteyttä Azure virtual verkon lisäämällä voi laajentaa paikalliseen verkkoon Azure turvallisesti. Valitse VPN-yhteyden avulla tietoliikenteestä salataan ja reitittää Internetin kautta. Tässä esimerkissä NVA on Pakota ja hallita suojauskäytäntö keskitetysti. Lisätietoja on artikkelissa muodosta yksityiskohtaisia ohjeita (tarkastuksen). Nämä ohjeet ovat seuraavat:

- Miten voit luoda tämän esimerkin ympäröivän verkon PowerShell-komentosarjojen kanssa.
- Miten voit luoda tämän esimerkin Azure Resurssienhallinta-malli.
- Yksityiskohtainen liikenne sähköpostiskenaarioiden määrittämisessä, näyttää, miten liikenne kulkee tämän rakenne.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Esimerkki 5: Lisää hybrid sivusto sivusto, Azure yhdyskäytävän VPN-yhteyttä
[Takaisin alkuun nopeasti](#fast-start) | Tarkat muodosta ovat käytettävissä pian

![Ympäröivän verkon yhdyskäytävän yhdistetyn hybrid verkoston kanssa][14]

#### <a name="environment-description"></a>Ympäristön kuvaus
Hybrid verkko Azure VPN-yhdyskäytävän avulla voidaan lisätä joko ympäröivän verkkotyyppi on kuvattu esimerkkejä 1 tai 2.

Edeltävässä kuvassa esitetyllä Internetin välityksellä (sivuston sivuston) VPN-yhteyden avulla luodaan yhteys paikalliseen verkkoon Azure virtual verkkoon Azure VPN-yhdyskäytävän kautta.

>[AZURE.NOTE] Jos käytät ExpressRoute Azure julkisen Peering-asetus on otettu käyttöön, luodaanko staattisen reitin. Tämä reitittää NVA VPN-IP-osoitteeseen yrityksen Internet- ja ei kautta ExpressRoute WAN. Pakollinen ExpressRoute Azure julkisen Peering-asetus NAT voivat katkaista VPN-istunnon.

Seuraavassa kuvassa on kaksi verkon reunat-asetus. Ensimmäinen reunan NVA ja NSGs ohjaavat liikenne kulkee sisäiset Azure-verkoissa ja Azure ja Internetin välillä. Toinen reuna on Azure VPN-yhdyskäytävän, joka on kokonaan erillinen ja eristetty verkko-reuna paikallisen ja Azure välillä.

Liikenne kulkee tulee pitää huolellisesti, ne voidaan optimoida tai heikentynyt mukaan rakenne tätä mallia mukaan erityiset käyttötapaus.

Esimerkki 1: n valmista ympäristön avulla ja sitten lisätä sivuston sivuston VPN hybrid on verkkoyhteys tuottaa seuraavat rakenne:

![Ympäröivän verkon kanssa yhdyskäytävän ExpressRoute-yhteyden avulla][15]

#### <a name="conclusion"></a>Tekemistä
Sivuston sivuston VPN hybrid verkkoyhteyttä Azure virtual verkon lisäämällä voi laajentaa paikalliseen verkkoon Azure turvallisesti. Käytä alkuperäisen Azure VPN-yhdyskäytävän, tietoliikenteestä on salattu IP ja reitittää Internetin kautta. Myös Azure VPN-yhdyskäytävän avulla voit antaa alemman kustannukset-vaihtoehto (ei ole muita käyttöoikeudet kustannusten kolmannen osapuolen NVAs kanssa). Tämä on eniten taloudellisia esimerkissä 1, jossa ei ole NVA käytetään. Lisätietoja on artikkelissa muodosta yksityiskohtaisia ohjeita (tarkastuksen). Nämä ohjeet ovat seuraavat:

- Miten voit luoda tämän esimerkin ympäröivän verkon PowerShell-komentosarjojen kanssa.
- Miten voit luoda tämän esimerkin Azure Resurssienhallinta-malli.
- Yksityiskohtainen liikenne sähköpostiskenaarioiden määrittämisessä, näyttää, miten liikenne kulkee tämän rakenne.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Esimerkki 6: Lisää ExpressRoute hybrid yhteyden
[Takaisin alkuun nopeasti](#fast-start) | Tarkat muodosta ovat käytettävissä pian

![Ympäröivän verkon yhdyskäytävän yhdistetyn hybrid verkoston kanssa][16]

#### <a name="environment-description"></a>Ympäristön kuvaus
Hybrid verkko-ExpressRoute yksityinen peering yhteyden avulla voidaan lisätä joko ympäröivän verkkotyyppi on kuvattu esimerkkejä 1 tai 2.

Edeltävässä kuvassa esitetyllä ExpressRoute yksityinen peering on suora paikallisen verkon ja Azure virtual verkon välinen yhteys. Liikenne transits palvelun palveluntarjoajan verkkoon ja Microsoft Azure-verkko-koskettaa koskaan Internetissä.

>[AZURE.NOTE] Ole tiettyjä rajoituksia, kun käytät UDR ExpressRoute, dynaaminen reititys käyttää Azure virtual yhdyskäytävän monimutkaisuuden vuoksi. Nämä ovat seuraavat:
>
>- UDR käytetään ei Gatewayn aliverkon, joina ExpressRoute linkitetyn Azure virtual yhdyskäytävä on yhdistetty.
>- ExpressRoute linkitetyn Azure virtual yhdyskäytävä ei voi olla muita UDR NextHop laitteen sidottu aliverkosta.
>
>
<br />

>[AZURE.TIP] ExpressRoute seuraa yrityksen verkkoliikennettä tietoturvasyistä Internet-luettelosta ja kasvoi merkittävästi suorituskykyä. Sen avulla voidaan myös service level Agreement ExpressRoute-palveluntarjoajalta. Azure-yhdyskäytävän siirtää enintään 2 gt/s ExpressRoute-sivusto sivusto VPN-yhteydet ja Azure yhdyskäytävän suurin nopeus on 200 Mt/s.

Seuraavassa kaaviossa tarkastelu tällä asetuksella ympäristöön on nyt kaksi verkon reunoja. NVA ja NSG ohjaavat sisäiset Azure verkoissa ja Azure – Internet-liikenne kulkee yhdyskäytävän ollessa kokonaan erillinen ja eristetty verkko-reuna paikallisen ja Azure välillä.

Liikenne kulkee tulee pitää huolellisesti, ne voidaan optimoida tai heikentynyt mukaan rakenne tätä mallia mukaan erityiset käyttötapaus.

Esimerkki 1: n valmista ympäristön avulla sekä lisäämällä ExpressRoute hybrid verkkoyhteyden tilan, tuottaa seuraavat rakenne:

![Ympäröivän verkon kanssa yhdyskäytävän ExpressRoute-yhteyden avulla][17]

#### <a name="conclusion"></a>Tekemistä
Lisäämällä ExpressRoute yksityinen Peering-verkkoyhteys voi laajentaa paikalliseen verkkoon Azure-suojatun, pienet viive-suorittamiseen suurempi tavalla. Myös alkuperäinen Azure yhdyskäytävää, kuten tässä esimerkissä sisältävät alemman kustannukset vaihtoehto (ei muut kolmannen osapuolen NVAs kanssa käyttöoikeudet). Lisätietoja on artikkelissa muodosta yksityiskohtaisia ohjeita (tarkastuksen). Nämä ohjeet ovat seuraavat:

- Miten voit luoda tämän esimerkin ympäröivän verkon PowerShell-komentosarjojen kanssa.
- Miten voit luoda tämän esimerkin Azure Resurssienhallinta-malli.
- Yksityiskohtainen liikenne sähköpostiskenaarioiden määrittämisessä, näyttää, miten liikenne kulkee tämän rakenne.

## <a name="references"></a>Viittaukset
### <a name="helpful-websites-and-documentation"></a>Hyödyllisiä sivustoja ja asiakirjat
- Accessin Azure ja Azure resurssin Manager:
- Azure PowerShellin käyttäminen: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Virtuaalinen verkko dokumentaatio: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Verkon suojaus ryhmän dokumentaatio: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Käyttäjän määrittämät reititys dokumentaatio: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure virtual yhdyskäytävien: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- Sivuston sivuston VPN-yhteydet: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- ExpressRoute asiakirjat (olla "Aloittaminen" ja "How To"-osissa Tarkista): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Suojausasetusten vuokaavio"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure yhteensopivuuden merkkejä"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Azure suojaustoiminnot"
[3]: ./media/best-practices-network-security/dmzcorporate.png "DMZ yrityksen verkossa"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Azure suojausarkkitehtuuri"
[5]: ./media/best-practices-network-security/dmzazure.png "DMZ Azure Virtual verkossa"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hybrid verkossa, jossa on kolme suojauksen rajat"
[7]: ./media/best-practices-network-security/example1design.png "Saapuvien DMZ NSG kanssa"
[8]: ./media/best-practices-network-security/example2design.png "Saapuvan DMZ NVA ja NSG"
[9]: ./media/best-practices-network-security/example3design.png "Kaksisuuntaisen DMZ NVA, NSG ja UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Palomuurisäännöt loogisten näkymä"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ kanssa NVA yhdistetty Hybrid verkossa"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ liitetty sivuston sivuston VPN NVA kanssa"
[13]: ./media/best-practices-network-security/example4networklogical.png "Looginen verkko NVA näkökulmasta"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ Azure Gatewayn liitetty sivuston sivuston Hybrid verkossa"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ Azure Gatewayn sivusto sivusto VPN-yhteyttä käyttämällä"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ Azure Gatewayn yhdistetty ExpressRoute Hybrid verkossa"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ Azure Gatewayn ExpressRoute-yhteyden avulla"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
