<properties
   pageTitle="Reititys vaatimukset ExpressRoute | Microsoft Azure"
   description="Tällä sivulla on vaatimukset määrittämisestä ja hallinnasta ExpressRoute piirit reititys."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>ExpressRoute reititys vaatimukset  

Tarvitset muodostaa yhteyttä Microsoftin pilvipalveluihin ExpressRoute avulla voit määrittää ja hallita reititys. Jotkin yhdistämispalvelua tarjoajat tarjoavat määrittämisestä ja hallinnasta reititys hallitun palveluna. Tarkista yhteys palvelussa nähdäksesi, jos ne tarjota tämä palvelu. Jos ne eivät sinun on noudatettava seuraavien ehtojen mukaan. 

Lisätietoja [piirit ja reititys toimialueiden](expressroute-circuit-peerings.md) artikkelista reititys istunnoista, jotka on määritettävä helpottamiseksi connectivity kuvauksia.

>[AZURE.NOTE] Microsoft ei tue käyttömahdollisuudet suuren käytettävyyden minkä tahansa reitittimen redundancy protokollat (esimerkiksi HSRP, VRRP). Olemme luottavat erityisen istuntojen kohden peering suuren tarpeettomat kahdet.

## <a name="ip-addresses-used-for-peerings"></a>Käytetään peerings IP-osoitteet

Haluat varata muutaman lohkot IP-osoitteiden määrittäminen reititys verkon ja Microsoftin yrityksen reuna (MSEEs) reitittimen välillä. Tässä osassa on luettelo vaatimukset ja kuvataan, kuinka nämä IP-osoitteet on hankittu ja käyttää koskevat säännöt.

### <a name="ip-addresses-used-for-azure-private-peering"></a>Käytettävä Azure yksityinen peering IP-osoitteet

Voit käyttää yksityisten IP-osoitteiden tai julkisten IP-osoitteiden määrittäminen peerings. Käytettävä määrittäminen tiet osoitealueita on päällekkäin avulla luodaan virtual verkkojen Azure osoitealueet. 

 - On varata /29 aliverkon tai kaksi /30 aliverkosta reititys liittymät varten.
 - Käytettävä reititys aliverkosta voi olla yksityinen IP-osoitteet tai julkiseen IP-osoitteet.
 - Aliverkosta eivät saa olla ristiriidassa alueen varattu käytettäväksi Microsoft-pilvipalvelussa asiakkaan kanssa.
 - Jos /29 aliverkon käytetään, se jaetaan kaksi /30 aliverkosta. 
     - Ensimmäinen /30 aliverkon käytetään ensisijainen linkki ja toinen/30 aliverkon käytetään toissijainen linkin.
     - Kunkin /30 aliverkosta, sinun on käytettävä ensimmäisen IP-osoite /30 aliverkon reitittimessä. Microsoft käyttää toisen IP-osoite /30 aliverkon erityisen istunnon määrittäminen.
     - Määritettävä sekä erityisen istuntojen Microsoftin [käytettävyyttä SLA](https://azure.microsoft.com/support/legal/sla/) on voimassa.  

#### <a name="example-for-private-peering"></a>Yksityinen peering-Esimerkki

Jos haluat määrittää peering a.b.c.d/29 avulla, se jaetaan kaksi /30 aliverkosta. Seuraavassa esimerkissä on näyttää, miten a.b.c.d/29 aliverkon käytetään. 

a.b.c.d/29 jakaa a.b.c.d/30 ja a.b.c.d+4/30 ja Microsoft kerää valmistelu ohjelmointirajapinnan siirretään. Sinun on käytettävä a.b.c.d+1 VRF IP sellaisten ensisijainen k2 ja Microsoft käyttää a.b.c.d+2 kuin VRF IP ensisijainen MSEE varten. Sinun on käytettävä a.b.c.d+5 VRF IP sellaisten toissijainen k2 ja Microsoft käyttää a.b.c.d+6 VRF IP toissijainen MSEE varten.

Harkitse palvelupyynnön, jossa voit määrittää yksityinen peering 192.168.100.128/29 valita. 192.168.100.128/29 sisältää osoitteiden 192.168.100.128 192.168.100.135, kuten:

- 192.168.100.128/30 määritetään link1-palvelussa 192.168.100.129 ja Microsoft 192.168.100.130 avulla.
- 192.168.100.132/30 määritetään link2-palvelussa 192.168.100.133 ja Microsoft 192.168.100.134 avulla.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>Azure julkinen ja Microsoft peering käytettävän IP-osoitteet

Sinun on käytettävä julkiseen IP-osoitteet, jotka omistat erityisen istunnot määrittämisestä. Microsoft on voitava muodostaa omistajuuden reititys Internet-rekistereihin ja Internet-reititys rekistereihin IP-osoitteet. 

- Käytössä on oltava yksilöllinen/29 aliverkon tai kaksi /30 aliverkosta, kunkin peering kohti ExpressRoute peering erityisen määrittämään piiri (Jos sinulla on useampi kuin yksi). 
- Jos /29 aliverkon käytetään, se jaetaan kaksi /30 aliverkosta. 
    - Ensimmäinen /30 aliverkon käytetään ensisijainen linkki ja toinen/30 aliverkon käytetään toissijainen linkin.
    - Kunkin /30 aliverkosta, sinun on käytettävä ensimmäisen IP-osoite /30 aliverkon reitittimessä. Microsoft käyttää toisen IP-osoite /30 aliverkon erityisen istunnon määrittäminen.
    - Määritettävä sekä erityisen istuntojen Microsoftin [käytettävyyttä SLA](https://azure.microsoft.com/support/legal/sla/) on voimassa.

## <a name="public-ip-address-requirement"></a>Pakollisen julkiseen IP-osoite 

### <a name="private-peering"></a>Yksityinen Peering 

Voit käyttää yksityinen peering julkinen tai yksityinen IPv4-osoitteet. Annamme lopusta loppuun eristystaso tietoliikenteestä, että päällekkäinen muiden asiakkaiden osoitteita ei ole mahdollista, jos kyseessä on yksityinen peering. Nämä osoitteet ei ilmoittaa Internet-yhteys. 

### <a name="public-peering"></a>Julkisen Peering

Azure julkisen peering polku avulla voit muodostaa yhteyden kaikkiin palveluihin ylläpidettävä Azure niiden julkisten IP-osoitteiden päälle. Näihin kuuluvat palveluja [ExpessRoute usein kysytyt kysymykset](expressroute-faqs.md) ja ylläpitää valmistajille Microsoft Azure-palvelut. Yhteys Microsoft Azure palvelut julkisen peering aloitetaan aina verkosta Microsoft-verkostoon. Sinun on käytettävä julkiseen IP-osoitteiden tarkoitettu Microsoft verkon tietoliikenteen.

### <a name="microsoft-peering"></a>Microsoft Peering

Microsoft peering polku avulla voit muodostaa yhteyttä Microsoftin pilvipalveluihin, joita ei tueta Azure julkisen peering polun. Luettelo palveluista on Office 365-palveluja, kuten Exchange Onlinen, SharePoint online-tilassa, Skype for Business-sovelluksen ja CRM Online. Microsoft tukee kaksisuuntaisen yhteyden Microsoft peering. Microsoftin pilvipalveluihin tarkoitettu liikenne on käytettävä kelvollinen julkisen IPv4-osoitteet, ennen kuin he kirjoittavat Microsoft network.

Varmista, että IP-osoite ja kuin number sinulle jossakin alla paikallisrekisterit rekisteröity.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Julkisten IP-osoitteiden ilmoitetaan Microsoft ExpressRoute päälle ei on julkaistava Internet. Tämä saattaa katkaista muiden Microsoftin palvelujen yhteys. Verkoston palvelimissa, jotka O365 päätepisteet Microsoft yhteydessä käyttämien julkiseen IP-osoitteet saattavat kuitenkin määrittämiisi ExpressRoute päälle. 

## <a name="dynamic-route-exchange"></a>Dynaaminen reititys exchange

Reititys exchange on eBGP-protokollan kautta. EBGP istuntojen muodostetaan MSEEs ja yhteyttä reitittimen välillä. Todennus erityisen istuntojen ei tarvita. Tarvittaessa MD5 hash voi määrittää. Saat lisätietoja erityisen istuntojen määrittäminen [Määritä reititys](expressroute-howto-routing-classic.md) ja [piiri valmistelu työnkulut ja piiri hyötyä](expressroute-workflows.md) .

## <a name="autonomous-system-numbers"></a>Yksipuolisia järjestelmän numerot

Microsoft käyttää AS 12076 Azure julkinen, yksityinen Azure ja Microsoft peering. Microsoft on varattu lähetysilmoitukset 65515, 65520 sisäiseen käyttöön. 16 ja 32-bittinen LUKUINA ovat tuettuja.

Ei ole vaatimukset tietojen siirto symmetrian ympärille. Eteenpäin ja palaa polut voi siirtää eri reitittimen parit. Identtiset tiet on julkaistava joko puolilta useita piiri paria kuuluvat voit yli. Reitin arvot ei tarvitse olla sama.

## <a name="route-aggregation-and-prefix-limits"></a>Reitittää kooste ja etuliite rajoitukset

Sovellus tukee enintään 4 000 etuliitteiden määrittämiisi US Azure yksityinen peering kautta. Tämä voidaan lisätä enintään 10 000 etuliitteiden Jos ExpressRoute premium-lisäosa on otettu käyttöön. Syy hyväksyy enintään 200 etuliitteiden Azure julkisen erityisen istunnossa ja Microsoft peering. 

Erityisen istunnon jätetään pois, jos etuliitteiden määrä ylittää. Syy hyväksyy oletusarvon tiet vain yksityinen peering linkkiä. Palvelu on suodatettava oletusarvon reitin ja yksityiset IP-osoitteet (RFC 1918) julkisen azuren ja Microsoft peering polkuja. 

## <a name="transit-routing-and-cross-region-routing"></a>Salataanko siirrettävät reititys ja rajat-alueen reititys

ExpressRoute ei voi määrittää salataanko siirrettävät reitittimen. Sinun on yhteys-palveluntarjoajan salataanko siirrettävät reititys-palveluiden riippuvaisia.

## <a name="advertising-default-routes"></a>Mainonta oletusarvon tiet

Oletusarvoinen tiet sallitaan vain Azure yksityinen peering istuntoja. Siinä tapauksessa että reitittää kaikki liikenne liittyvät virtual verkoista verkkoon. Mainonta oletusarvon tiet kyselyjä yksityinen peering johtaa internet-polun azuren ei estettäisi. Tarvitset on kohdassa yrityksen reuna ja haluat reitittää liikenteen- ja Azure maksuttomien palveluiden Internet-yhteys. 

 Varmista ottamaan yhteys muihin Azure services ja infrastruktuuripalvelut on määritetty jokin seuraavista:

 - Azure julkisen peering on käytössä reititys-liikenne paikalliseen julkisen päätepisteet
 - Salli internet-yhteys, joka aliverkon edellyttävät Internet-yhteys käyttäjän määrittämä reititys avulla.
 
>[AZURE.NOTE] Mainonta oletusarvon tiet katkaista Windows ja muut AM käyttöoikeuden aktivointi. Ohjeiden [Tässä](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) voit ratkaista ongelman.

## <a name="support-for-bgp-communities-preview"></a>Tuki erityisen yhteisöjen (ennakkoversio)


Tässä osassa on yleiskatsaus siitä, miten erityisen yhteisöjen käytetään ExpressRoute. Microsoft ilmoittaa tiet julkinen ja Microsoft peering polut tiet, jotka on merkitty asianmukaiset yhteisön arvojen kanssa. Näin saat perusteet ja yhteisön arvojen tiedot ovat jäljempänä. Microsoft-kuitenkin ei tukee merkityt tiet ilmoitetaan Microsoft yhteisön arvoja.

Jos yrität muodostaa yhteyden Microsoft ExpressRoute yhden peering sijainnissa geopoliittisten alueella kautta, sinun on pääsy kaikki Microsoftin pilvipalveluihin geopoliittisten rajojen sisällä kaikkien alueiden välillä. 

Esimerkiksi jos yhteys Microsoft Amsterdamissa ExpressRoute kautta, sinun on pääsy kaikki ylläpidettävä Pohjois Eurooppa ja Länsi Europe Microsoftin pilvipalveluihin. 

Lisätietoja on ohjeaiheessa [ExpressRoute kumppanit ja peering sijainnit](expressroute-locations.md) -sivulla yksityiskohtainen luettelo geopoliittisten alueiden, liitetty Azure alueiden ja vastaavan ExpressRoute peering sijainnit.

Voit hankkia useita ExpressRoute piiri geopoliittisten alueittain. On useita yhteyksiä on hyvin käytettävyyden vuoksi geo redundancy merkittäviä etuja. Jos sinulla on useita ExpressRoute piirit tapauksissa saat etuliitteiden määrittämiisi Microsoft julkisen peering ja Microsoft peering polut samat. Tämä tarkoittaa sitä, sinulla on useita polkuja Microsoft verkosta. Tämä aiheuttaa mahdollisesti optimaalisen reititys päätöksiä verkossa oleville tehdään. Tämän vuoksi saattaa ilmetä optimaalisen connectivity kokemukset eri palveluihin. 

Microsoft tunnisteen etuliitteiden määrittämiisi – julkisen peering ja Microsoft peering tarvittavat erityisen yhteisön arvot aluetta etuliitteitä isännöidään. Voit luottaa yhteisön arvot-päätösten tarvittavat reititys tarjota [näin ollen optimaalisten reititys asiakkaille](expressroute-optimize-routing.md).

| **Geopoliittisten alue** | **Microsoft Azure-alue** | **Erityisen yhteisön arvo** |
|---|---|---|
| **Pohjois-Amerikka** |    |  |
|    | Yhdysvaltojen Itä | 12076:51004 |
|    | Yhdysvaltojen Itä 2 | 12076:51005 |
|    | Länsi USA | 12076:51006 |
|    | Länsi US 2 | 12076:51026 |
|    | Länsi keskitetyn USA | 12076:51027 |
|    | Pohjois-keskitetyn USA | 12076:51007 |
|    | Etelä keskitetyn USA | 12076:51008 |
|    | Keskitetyn USA | 12076:51009 |
|    | Kanada Keski | 12076:51020 |
|    | Kanada Itä | 12076:51021 |
| **Etelä-Amerikka** |  |  |
|    | Brasilia Etelä | 12076:51014 |
| **Europe** |    |  |
|    | Pohjois-Eurooppa | 12076:51003 |
|    | Länsi Europe | 12076:51002 |
| **Aasian ja Tyynenmeren alueen** |    |   |
|    | Itä-Aasian | 12076:51010 |
|    | Kaakkoisaasialaiset Aasian | 12076:51011 |
| **Japani** |     |   |
|    | Japani Itä | 12076:51012 |
|    | Japani Länsi | 12076:51013 |
| **Australia** |    |   | 
|    | Australia Itä | 12076:51015 |
|    | Australia varaaja | 12076:51016 |
| **Intia** |    |   |
|    | Intia Etelä | 12076:51019 |
|    | Intia Länsi | 12076:51018 |
|    | Intia Keski | 12076:51017 |

Kaikki tiet määrittämiisi Microsoftin merkitty asianmukaiset yhteisön-arvo. 

>[AZURE.IMPORTANT] Yleinen etuliite asianmukaiset yhteisön arvo merkitty ja ilmoitetaan, vain silloin, kun ExpressRoute premium-lisäosa on otettu käyttöön.


Lisäksi edellä Microsoft myös tunnisteen etuliitteiden perustuvan ne kuuluvat. Tämä koskee vain Microsoft peering. Seuraavassa taulukossa on erityisen yhteisön arvo palvelun määritystä.

| **Palvelun** | **Erityisen yhteisön arvo** |
|---|---|
| **Exchange** | 12076:5010 |
| **SharePoint** | 12076:5020 |
| **Skype For Business** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Muut Office 365-palvelut** | 12076:5100 |

>[AZURE.NOTE] Microsoft ei ota huomioon erityisen yhteisön-arvoja, jotka voit määrittää Microsoft ilmoitetaan tiet.

## <a name="next-steps"></a>Seuraavat vaiheet

- Määritä ExpressRoute-yhteys.

    - [Luo ExpressRoute-piiri perinteinen käyttöönotto-mallin](expressroute-howto-circuit-classic.md) tai [Luo ja Muokkaa Azure resurssien hallinnan ExpressRoute-piiri](expressroute-howto-circuit-arm.md)
    - [Määritä reititys perinteinen käyttöönotto-mallin](expressroute-howto-routing-classic.md) tai [Määritä reititys resurssien hallinnan käyttöönottomalli](expressroute-howto-routing-arm.md)
    - [Perinteinen VNet ExpressRoute piiri,](expressroute-howto-linkvnet-classic.md) tai [linkkiä Resurssienhallinta-VNet, ExpressRoute piiri](expressroute-howto-linkvnet-arm.md)


