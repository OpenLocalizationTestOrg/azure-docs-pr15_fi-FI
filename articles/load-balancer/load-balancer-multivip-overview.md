<properties
   pageTitle="Useita VIPs varten Azure kuormituksen | Microsoft Azure"
   description="Valitse Azure kuormituksen useita VIPs yleiskatsaus"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Useita VIPs varten Azure kuormituksen

Azure ladata tasaustoiminto voit ladata useita portit ja/tai useita IP-osoitteita saldo palvelut. Voit ladata saldo kulkee joukon VMs julkinen ja sisäisistä kuormituksen tasauspalvelun määritykset.

Tässä artikkelissa kuvataan tämän ominaisuuden, tärkeitä käsitteitä ja rajoitusten perusteet. Jos haluat vain näyttää palvelut yksi IP-osoite, löydät yksinkertaistettu ohjeet [julkinen](load-balancer-get-started-internet-portal.md) tai [sisäinen](load-balancer-get-started-ilb-arm-portal.md) ladata tasaustoiminto määrityksiä. Lisäämällä useita VIPs on varmasti yhden VIP-määritys. Tässä artikkelissa kuvattujen käsitteitä avulla voit laajentaa yksinkertaistettu määritysten milloin tahansa.

Azure-kuormituksen määrittäessäsi frontend ja Taustajärjestelmä määritys on yhdistetty säännöillä. Sääntö viittaa kunto näytteenottimen käytetään uusien kulkee lähetetään solmun Taustajärjestelmä sarjassa. Frontend on määritetty mukaan Virtual IP (VIP), joka on 3-monikon koostuu IP-osoite (yleinen tai sisäinen), transport protocol (UDP tai TCP) ja portin numeroa. DIP on liitetty AM Taustajärjestelmä varannon Azure virtual NIC IP-osoitteen.

Seuraavassa taulukossa on joitakin Esimerkki edusta-määritykset:

| VIP | IP-osoite | protokolla | Port (portti) |
|-----|------------|----------|------|
|1|65.52.0.1|TCP|80|
|2|65.52.0.1|TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|TCP|80|

Taulukossa on neljä eri frontends. Frontends #1, #2 ja #3 on yksittäinen VIP useita sääntöjä. Sama IP-osoite on käytössä, mutta portti tai protokolla on erilainen jokaiselle frontend. Frontends #1 ja #4 ovat esimerkiksi useita VIPs, jossa samaa edusta-protokollaa ja porttia käytetään uudelleen useita VIPs yli.

Azure ladata tasaustoiminto joustavasti määrittäminen kuormituksen säännöt. Säännön ilmoittaa, kuinka osoite ja edusta-porttiin on yhdistetty kohdeosoite ja portin taustassa. Onko Taustajärjestelmä portit uudelleen säännöt koko määräytyy säännön tyypin. Kunkin sääntö on erityiset vaatimukset, jotka voivat vaikuttaa isännän kokoonpano ja näytteenottimen rakenne. On kahdentyyppisiä sääntöjä:

1. Oletusarvo-sääntöä, jonka ei ole Taustajärjestelmä portin uudelleenkäyttö
2. Irrallinen IP säännön, jossa Taustajärjestelmä portit käytetään uudelleen

Azure ladata tasaustoiminto voit sekoita molemmat samaan kuormituksen tasauspalvelun määritysten sääntötyypeistä. Kuormituksen voit käyttää niitä samanaikaisesti annetun AM tai mikä tahansa yhdistelmä, kunhan asiakas noudata säännön rajoitukset. Valitse säännön tyyppi määräytyy sovelluksen vaatimukset ja monimutkaisuudesta tukevat kyseisen määritys. On arvioitava, mitkä sääntötyypeistä sopivat parhaiten käyttämässäsi skenaariossa.

Näissä tilanteissa tutkiminen oletustoiminnan alkaen.

## <a name="rule-type-1-no-backend-port-reuse"></a>Säännön tyyppi #1: ei Taustajärjestelmä portin uudelleenkäyttö

![MultiVIP kuva](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

Tässä skenaariossa edusta-VIPs määritetään seuraavasti:

| VIP | IP-osoite | protokolla | Port (portti) |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

DIP on saapuvan työnkulku kohteen. Kunkin AM paljastaa Taustajärjestelmä sarjassa DIP yksilöllinen porttiin haluamasi huolto. Tämä palvelu on liitetty edusta säännön määritys kautta.

Olemme määrittää kahden sääntöjä:

| Sääntö | Yhdistä frontend | Taustajärjestelmä ryhmään |
|------|--------------|-----------------|
| 1 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![taustaan](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![taustaan](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![taustaan](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![taustaan](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Yhdistäminen-Azure kuormituksen on nyt seuraavasti:

| Sääntö | VIP IP-osoite | protokolla | Port (portti) | Kohde | Port (portti) |
|------|----------------|----------|------|-----|------|
|![sääntö](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|UPOTETAAN IP-osoite|80|
|![sääntö](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|UPOTETAAN IP-osoite|81|

Niiden sääntöjen täytyy tuottaa työnkulun yksilöllinen yhdistelmän kohde-IP-osoite ja kohdeportti. Vaihteleva kulun Kohdeportti useita sääntöjä voit toimittaa kulkee, sama DIP eri porttiin.

Kuntotietojen keräysputkien ohjataan aina AM DIP. Sinun on varmistettava, että näytteenottimen kuvastaa AM kunto.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Säännön tyyppi #2: Taustajärjestelmä portin uudelleenkäyttöä irrallinen IP käyttämällä

Azure kuormituksen tarjoaa joustavuutta uudelleen edusta-portin yli useita VIPs käytetään sääntötyyppi riippumatta. Lisäksi sovelluksen joissakin tilanteissa mieluummin tai vaadi samaa porttia käytettävän useita Palvelusovellusten esiintymät, valitse yksittäinen AM Taustajärjestelmä sarjassa. Yleisiä esimerkkejä portin uudelleenkäyttöä Sisällytä käyttövarmuuden lisäämiseksi suuren käytettävyyden, verkon virtual laitteiden ja paljastaa useita TLS päätepisteet ilman uudelleen salausta.

Jos haluat käyttää Taustajärjestelmä portti yli useita sääntöjä, sinun on otettava irrallinen IP säännön määritys.

Irrallinen IP on osa mitä kutsutaan nimellä suoraan palvelimen palauttaa (DSR). DSR koostuu kahdesta: työnkulku topologian ja IP sähköpostiosoitteen yhdistäminen värimallin. Ympäristön tasolla Azure kuormituksen toimii aina DSR-työnkulku topologian riippumatta siitä, onko irrallinen IP käytössä vai ei. Tämä tarkoittaa, että kirjoitettava uudelleen vuo lähtevä osa aina oikein kulkevan suoraan takaisin alkuperän.

Oletus-sääntötyyppi kanssa Azure paljastaa perinteinen kuormituksen helppous IP address yhdistäminen malli. IP address yhdistäminen värimallin sallimaan joustavasti alla kuvatulla ottaminen käyttöön irrallinen IP muuttuu.

Seuraavassa kaaviossa on kuvattu nämä määritykset:

![MultiVIP kuva](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

Tässä skenaariossa jokaisen AM Taustajärjestelmä sarjassa on kolme verkkoliittymät:

* DIP: Virtual NIC liittyvät AM (Azure's NIC resurssi)
* VIP1: silmukan käyttöliittymä Vieras OS, joka on määritetty IP-osoite VIP1 sisällä
* VIP2: silmukan käyttöliittymä Vieras OS, joka on määritetty IP-osoite VIP2 sisällä

>[AZURE.IMPORTANT] Loogisen liittymät määritysten suoriteta Vieras OS. Tässä määrityksessä suorittaa tai hallitsee Azure ei. Tässä määrityksessä ilman sääntöjä ei toimi. Kuntotietojen näytteenottimen määritelmät käyttää DIP AM looginen VIP sijaan. Vuoksi palvelussa on määritettävä näytteenottimen vastaukset DIP porttiin, joka vastaa tarjota palvelun tila-looginen VIP.

Oletetaan sama edusta kokoonpano, kuten edellisen skenaariota:

| VIP | IP-osoite | protokolla | Port (portti) |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

Olemme määrittää kahden sääntöjä:

| Sääntö | Yhdistä frontend | Taustajärjestelmä ryhmään |
|------|--------------|-----------------|
| 1 | ![sääntö](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![taustaan](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (VM1 ja VM2) |
| 2 | ![sääntö](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![taustaan](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (VM1 ja VM2) |

Seuraavassa taulukossa on valmis yhdistäminen kuormituksen:

| Sääntö | VIP IP-osoite | protokolla | Port (portti) | Kohde | Port (portti) |
|------|----------------|----------|------|-------------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|sama kuin VIP (65.52.0.1)|sama kuin VIP (80)|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|sama kuin VIP (65.52.0.2)|sama kuin VIP (80)|

Saapuvien kulun kohde on AM silmukka-liittymän VIP-osoitteeseen. Niiden sääntöjen täytyy tuottaa työnkulun yksilöllinen yhdistelmän kohde-IP-osoite ja kohdeportti. Vaihteleva kulun kohde-IP-osoite, mukaan portin uudelleenkäyttöä on mahdollista saman AM. Palvelussa on määritetty kuormituksen sidonta VIP IP-osoite ja portin vastaaviin silmukka-liittymän avulla.

Huomaa, että tässä esimerkissä ei muuta kohdeportti. Vaikka tämä on kiinnitetty IP-skenaario, Azure kuormituksen tukee myös määrittämällä säännön uudelleen Taustajärjestelmä Kohdeportti ja tehdä eroaa edusta-kohdeportti.

Irrallinen IP-sääntötyyppi on useita kuormituksen tasauspalvelun määritysten kuviot foundation. Esimerkki, joka on tällä hetkellä käytettävissä on [Useita kuuntelijoita kanssa AlwaysOn SQL](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) -määritys. Ajan myötä on kuvattu seuraavista tilanteista.

## <a name="limitations"></a>Rajoitukset

* Useita VIP määrityksiä tuetaan vain IaaS VMs.
* Irrallinen IP-säännöllä sovelluksen on käytettävä DIP lähtevä kulkee. Jos sovellus sitoo määritetty Vieras OS silmukka-liittymän VIP-osoitteeseen, sitten SNAT ei ole käytettävissä korjaamaan lähtevä kulun ja työnkulku epäonnistuu.
* Julkisten IP-osoitteiden olla vaikutusta laskutus. Lisätietoja on artikkelissa [hinnoittelua IP-osoite](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Tilauksen raja-arvot koskevat. Lisätietoja on artikkelissa [palvelun rajoitukset](../azure-subscription-service-limits.md#networking-limits) lisätietoja.
