<properties 
   pageTitle="Virtuaalinen verkon VPN yhdyskäytävän usein kysytyt kysymykset | Microsoft Azure"
   description="VPN-yhdyskäytävän usein kysytyt kysymykset. Usein kysyttyjä Kysymyksiä Microsoft Azure Virtual paikallisen-verkkoyhteyksien hybrid määritysten yhteydet ja VPN yhdyskäytävät"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>VPN-yhdyskäytävän usein kysytyt kysymykset

## <a name="connecting-to-virtual-networks"></a>Yhteyden muodostaminen näennäisen verkkoihin

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Voit muodostaa eri Azure alueiden virtual verkot?
Kyllä. Ei itse asiassa alue ei rajoitetta. Yhden virtual verkoston muodostaa yhteyden toiseen virtual verkon samalla alueella tai Azure toisella alueella.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Voit muodostaa virtual verkot eri tilaukset?
Kyllä.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Voin muodostaa yhteyden usean sivuston yksittäisen virtual verkosta?

Voit muodostaa yhteyden useita sivustoja käyttämällä Windows PowerShellin ja Azure REST API. [Usean sivuston ja VNet VNet yhteyksien](#multi-site-and-vnet-to-vnet-connectivity) usein kysytyt kysymykset kohdassa.
## <a name="what-are-my-cross-premises-connection-options"></a>Mitkä ovat omat paikallisen-yhteyden asetukset?

Seuraavat paikallisen-yhteydet tuetaan:

- [Sivuston sivuston](vpn-gateway-site-to-site-create.md) – VPN-yhteyden kautta IP (IKE v1 ja IKE v2). Tämä yhteyden edellyttää VPN-laitetta tai RRAS.

- [Pisteen sivuston](vpn-gateway-point-to-site-create.md) – VPN-yhteyden kautta SSTP (Secure Socket Tunneling Protocol). Tätä yhteyttä ei edellytä VPN-laite.

- [VNet VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – tämä yhteystyyppi on sama kuin sivuston sivusto-määritys. VNet VNet on VPN-yhteyden kautta IP (IKE v1 ja IKE v2). Se ei edellytä VPN-laite.

- [Usean sivuston](vpn-gateway-multi-site.md) – tämä on muunnelma sivusto sivusto-määritys, jonka avulla voit yhdistää usean paikallisen sivuston virtual verkkoon.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute on Azure suoran yhteyden oman WAN-, ole julkisten Internetin välityksellä. Katso [ExpressRoute tekninen esittely](../expressroute/expressroute-introduction.md) ja lisätietoja [ExpressRoute usein kysytyt kysymykset](../expressroute/expressroute-faqs.md) .

Saat lisätietoja yhteyksiä [VPN yhdyskäytävä](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Mikä on sivuston sivuston yhteyden ja pisteen sivuston välinen ero?

**Sivuston sivuston** yhteydet avulla voit muodostaa välinen virtuaalikoneen tai rooli esiintymä virtual verkon sen mukaan, miten haluat määrittää reititys paikallisesti tietokoneista. Se on hyvä vaihtoehto aina käytettävissä olevien paikallisen yhteyden ja sovellu yhdistelmäympäristö. Yhteys on riippuvainen IP VPN laitteen (laitteiston tai Pehmeä laitteen), jotka on otettava käyttöön verkkoa reunan. Voit luoda tämän tyyppisen yhteyden, on oltava tarvittavat VPN-laitteet ja ulkoisesti aukeaman IPv4-osoite.

**Pisteen sivuston** yhteyksien avulla voit muodostaa yhteen tietokoneeseen missä tahansa lisääminen mihin tahansa virtual verkossa sijaitsevat. Se käyttää Windows-valinta VPN-asiakasohjelman. Osana pisteen sivuston määritykset asennat sertifikaatin ja VPN asiakasohjelman määritysten-paketti, joka sisältää asetuksia, joiden avulla tietokone muodostaa virtuaalikoneen ja-roolin esiintymän virtual verkoston. Se on hyvä, kun haluat muodostaa yhteyden virtual verkkoon, mutta ei sijaitsee paikallisen. Se on myös hyvä vaihtoehto, jos sinulla ei ole VPN laitteiston ja ulkoisesti aukeaman IPv4-osoite, jotka tarvitaan sivusto yhteyden. 

Virtuaalinen verkon käyttää sekä sivusto kohdan sivuston samanaikaisesti, voit määrittää, voit luoda sivuston sivuston yhteyden käyttämällä reitti-pohjainen VPN-tyyppi, että yhdyskäytävän. Reititys-pohjainen VPN-tyypit kutsutaan dynaaminen yhdyskäytävien perinteinen käyttöönotto-mallissa.

### <a name="what-is-expressroute"></a>Mikä on ExpressRoute?

ExpressRoute voit luoda yksityinen Microsoft palvelinkeskusten ja infrastruktuurin, joka on paikallisesti tai rinnakkain ympäristössä väliset yhteydet. ExpressRoute voit muodostaa yhteyksiä Microsoftin pilvipalveluihin esimerkiksi Microsoft Azure- ja Office 365: n ExpressRoute kumppanin rinnakkain-tilan osoitteessa tai suoraan yhteyden aiemmin WAN verkon (kuten MPLS VPN verkon palveluntarjoajan tarjoamat).

ExpressRoute yhteydet tarjoaa paremman suojauksen, Lisää luotettavuutta, suuremman kaistanleveyden ja pienempi viiveet suurempia kuin tavallinen yhteydet Internetin välityksellä. Joissakin tapauksissa ExpressRoute yhteyksien avulla voit siirtää tietoja paikallisen verkon ja Azure välillä voi myös tuotto kustannukset merkittäviä etuja. Jos olet jo luonut paikallisen-yhteyden paikallisen verkon Azure, voit siirtää ExpressRoute-yhteyden ja säilyttää virtual verkon ennalleen.

Katso lisätietoja [ExpressRoute usein kysytyt kysymykset](../expressroute/expressroute-faqs.md) .

## <a name="site-to-site-connections-and-vpn-devices"></a>Sivuston sivuston yhteydet ja VPN-laitteet

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Mitä tulee ottaa huomioon valittaessa VPN-laitteessa?

Olemme tarkistaneet vakio sivusto sivusto VPN-laitteiden yhteistyössä laitteen toimittajien kanssa. Luettelo tunnetuista yhteensopivat VPN-laitteet, niiden vastaavan määritysohjeet tai mallit ja laitteen Etkö löytyy [tähän](vpn-gateway-about-vpn-devices.md). Kaikki tunnetut yhteensopiva lueteltujen laitteen perheille, mikä pitäisi toimia Virtual verkoston kanssa. Auttamaan VPN-laitteen määrittäminen viittaavat laitteen määritysten otoksen tai linkkiä, joka vastaa sopiva laite perhe.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Mitä teen, jos käytössä on VPN-laitteella, joka ei ole yhteensopiva tunnetut laitteen luettelossa?

Jos et näe peruutuksesta tunnetut yhteensopiva VPN-laitteen laitteen ja haluat käyttää VPN-yhteyden, tarvitset varmistaa, että se täyttää tuettu IP/IKE määritysten asetukset ja parametrit luettelossa [tähän](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list). Vähimmäisvaatimukset täyttävät laitteet olisi toimivat hyvin VPN yhdyskäytävät. Pyydä laitteen valmistajalta tuki- ja lisäohjeita.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Miksi käytäntöön perustuva VPN-tunnelin Siirry kun liikenne ei käytetä?

Tämä on normaalia käytäntöön perustuva (tunnetaan myös nimellä staattinen reitittämiseen) VPN yhdyskäytävät. Kun tunnelin kautta liikenne ei käytetä enemmän kuin 5 minuuttia-tunnelin on poistettu. Kun liikenne alkaa juoksutus jompaankumpaan suuntaan, tunnelin muodostaa uudelleen heti.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Yhteyden muodostaminen Azure ohjelmiston VPN-yhteydet avulla?

Sovellus tukee Windows Server 2012 reitityksessä ja Remote Access (RRAS) palvelinten sivusto sivusto paikallisen-määritys.

Ohjelmiston VPN-ratkaisuja pitäisi toimia Microsoftin Gatewayn, kunhan alan vakio IP-käyttöotot mukaisiksi. Ohjelmiston toimittajalta määritys ja tuen ohjeet.

## <a name="point-to-site-connections"></a>Pisteen sivuston yhteydet

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Mitä käyttöjärjestelmiä pisteen sivuston kanssa voi käyttää?

Seuraavat käyttöjärjestelmät ovat tuettuja:

- Windows 7 (32-bittinen ja 64-bittinen)

- Windows Server 2008 R2 (vain 64-bittinen)

- Windows 8 (32-bittinen ja 64-bittinen)

- Windows 8.1 (32-bittinen ja 64-bittinen)

- Windows Server 2012 (vain 64-bittinen)

- Windows Server 2012 R2: n (vain 64-bittinen)

- Windows 10: ssä

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Mikä tahansa ohjelmiston VPN-asiakas käyttää piste-,-sivusto, joka tukee SSTP varten

Ei. Tuki on rajoitettu vain Windows-käyttöjärjestelmien versiot yllä olevassa luettelossa.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Kuinka monta VPN-asiakasohjelman päätepisteet pisteen sivuston-määritys voi olla?

Sovellus tukee jopa 128 VPN-asiakkaille, voivat muodostaa yhteyden virtual verkkoon samanaikaisesti.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Omaa sisäisen PKI varmenteiden päämyöntäjän käyttää pisteen sivuston yhteyksiä varten

Kyllä. Aiemmin voidaan käyttää vain itse allekirjoitettua päämyöntäjien varmenteet. Voit edelleen ladata 20 varmenteet.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Voin siirtää välityspalvelimet ja palomuurit on kohdassa sivuston ominaisuuksien avulla?

Kyllä. Käytämme palomuurit tunnelin SSTP (Secure Socket Tunneling Protocol). Tämä tunnelin näkyy HTTPs-yhteyden.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Jos määritetty pisteen sivustolla asiakastietokone käynnistää uudelleen, VPN-yhteys automaattisesti muodostaa?

Oletusarvon mukaan asiakastietokone ei muodostaa VPN-yhteyden automaattisesti.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Pisteen sivuston tukeen automaattinen-yhdistää ja DDNS VPN-asiakassovelluksissa?

Automaattinen muodosta ja DDNS tällä hetkellä ei tueta-kohdassa sivuston VPN-yhteydet.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Ovatko sivusto sivusto ja pisteen sivuston käyttömahdollisuudet olla sama virtual verkossa?

Kyllä. Molemmat vaihtoehdot toimivat, jos käytössäsi on käyttämäsi yhdyskäytävän RouteBased VPN-tyyppi. Perinteinen käyttöönottomalli sinun on dynaaminen yhdyskäytävä. Olemme tehdä ei tuki piste-,-sivusto staattinen reititys VPN-yhdyskäytävät tai yhdyskäytävät - VpnType PolicyBased avulla.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Voit määrittää pisteen sivuston asiakkaan useiden virtual verkostojen yhdistäminen samalla kertaa?

Kyllä, se on mahdollista. Mutta virtual verkkoja ei voi olla päällekkäiset IP etuliitteitä ja pisteen sivuston osoite välilyöntejä saa olla päällekkäiset virtual verkkojen välillä.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Kuinka paljon siirtonopeuden sivusto sivusto tai pisteen sivuston yhteyksien kautta voit odottaa?

On vaikeaa pitämään VPN-tunneleita tarkka siirtonopeuden. IP- ja SSTP ovat crypto näkyvä VPN-protokollia. Siirtonopeuden rajoittavat myös viivettä ja kaistanleveyttä asiakkaan tilojen ja Internetin välillä.

## <a name="gateways"></a>Yhdyskäytävät

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Mikä on yhdyskäytävän käytäntöön perustuva (staattinen reititys)?

Käytännön perustuva yhdyskäytävien Toteuta käytäntöön perustuva VPN-yhteydet. Käytännön perustuva VPN-yhteydet salaa ja suora pakettien IP tunnelissa osoitteiden etuliitteiden paikallisen verkon ja Azure VNet välille yhdistelmät perusteella. Käytännön (tai liikenne valitsin) määritetään yleensä VPN-määritys access-luettelossa.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Mikä on yhdyskäytävän reitti-pohjainen (dynaaminen reititys)?

Reititys-pohjainen yhdyskäytävien Toteuta reitti-pohjainen VPN-yhteydet. Reititys-pohjainen VPN-yhteydet käyttäminen "tiet" IP lähettäminen edelleen tai taulukon reitittämisestä suoraan pakettien niiden vastaavan tunnelin liittymät. Valitse tunnelin liittymät salaaminen ja salauksen ja siitä uloskirjautuminen akselitunnelien paketit. Käytännön tai liikenne rivinvalitsinta reitityksen VPN-yhteydet on määritetty kuin mikä tahansa-,-tahansa (tai yleismerkit).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Saan VPN yhdyskäytävän IP-osoite, ennen kuin se luodaan?

Ei. Sinun on luotava käyttämäsi yhdyskäytävän ensin Hae IP-osoite. Jos Poista ja luo VPN-yhdyskäytävän muuttuu IP-osoite.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Miten VPN-tunnelin Hae todennettu?

Azure VPN käyttää PSK (esijaettuja avain) todennusta. Olemme aiheuttaa VPN-tunnelin luodaan valmiiksi jaetun avaimen (PSK). Voit muuttaa automaattisesti luodut PSK haluat itse määrittää esijaettuja avain PowerShell cmdlet-komennon tai REST API.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Voinko käyttää määrittäminen esijaettuja avain Ohjelmointirajapinnan määrittämään oma käytäntöön perustuva (staattinen reititys) yhdyskäytävän VPN?

Kyllä, Määritä esijaettuja avain API ja PowerShell-cmdlet voidaan määrittää Azure (staattinen) VPN-käytäntö-pohjainen ja reititys-pohjainen (dynaamisen) reititys VPN.

### <a name="can-i-use-other-authentication-options"></a>Voinko käyttää muita todennusasetukset?

Microsoft on oikeutettu käyttämään jaettua näppäimet (PSK) todennusta varten.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Mikä on "yhdyskäytävän aliverkon" ja miksi sitä tarvitaan?

Gateway-palveluun, joka on suorittaa paikallisen-yhteys on. 

Sinun on luoda oman VNet määrittäminen VPN-yhdyskäytävän yhdyskäytävän aliverkon. Kaikki yhdyskäytävän aliverkosta nimen on oltava GatewaySubnet toimii oikein. Älä nimeä yhdyskäytävän aliverkon jotain muuta. Ja ei käyttöönottaminen VMs tai muu yhdyskäytävän aliverkon.

Yhdyskäytävän aliverkon vähimmäiskoko määräytyy määrityksistä, jotka haluat luoda. Se on mahdollista luoda yhdyskäytävän aliverkon jotkin määritykset /29 mahdollisimman pieni, mutta suosittelemme, että voit luoda yhdyskäytävän aliverkon /28 tai suurempi (/ 28 /27, /26 jne.). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Voin näennäiskoneiden tai rooli esiintymät käyttöön yhdyskäytävä-aliverkon?

Ei.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Miten joka määrittää liikenne käy läpi VPN-yhdyskäytävän?

Jos käytät perinteistä Azure-portaalissa, Lisää kunkin alueen, jonka haluat lähettää yhdyskäytävän kautta virtual verkon paikallisen verkkojen verkot-sivulla.

### <a name="can-i-configure-forced-tunneling"></a>Voin määrittää pakotettu Tunneling?

Kyllä. Kohdassa [Määritä pakotettu tunneling](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Voit määrittää Azure omaa VPN-palvelin ja käytä paikallista-verkkoon?

Kyllä, voit ottaa oman VPN yhdyskäytävien tai Azure Azure Marketplacesta tai luoda omia VPN-reitittimen palvelimiin. Sinun on käyttäjän määrittämä tiet paikallaan virtual verkossa, jotta liikenne reititetään oikein paikallisen lopettaminen ja VPN-aliverkosta välillä.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Miksi jotkin portit ovat avoinna VPN-yhdyskäytävän?

Niitä silti edellytetään Azure infrastruktuurin viestintään. Ne on suojattu (lukittu) mukaan Azure varmenteet. ERISNIMI varmenteet ilman ulkoisen kohteita, kuten asiakkaiden näiden yhdyskäytävää voi olla vaikutusta näiden päätepisteet johtuu.

VPN-yhdyskäytävä on olennaisesti usean hallittava laitetta, jonka yksi NIC napauttamalla asiakkaan yksityisverkon ja yksi Verkkokortti vastakkaisten yleiseen verkkoon. Azure infrastruktuurin kohteiden ei hyödynnä asiakkaan verkot yhteensopivuuden vuoksi, niin he tarvitsevat käyttämiseen julkisen päätepisteet infrastruktuurin viestintään. Julkinen päätepisteet säännöllisesti skannatut Azure suojauksen.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Lisätietoja yhdyskäytävän sekä vaatimukset ja siirtonopeuden

Lisätietoja on artikkelissa [Tietoja VPN yhdyskäytävän asetuksia](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Usean sivuston ja VNet VNet yhteydet

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Minkä tyyppisiä yhdyskäytävät tukevat usean sivuston ja VNet VNet yhteyksien?

Vain reitti-pohjainen VPN-yhteydet (dynaaminen reititys).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>VNet yhteyden RouteBased-VPN-sisältötyyppi, toinen VNet, PolicyBased VPN-tyyppi

Ei, sekä virtual verkot on käytettävä reitti-pohjainen (dynaaminen reititys) VPN-yhteydet.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>On VNet VNet liikenne suojattu?

Kyllä, se on suojattu IP/IKE salausta.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>VNet VNet liikenne kulkemaan Azure kuvaa runkoverkkoa?

Kyllä.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Kuinka monta paikallisen sivustot ja virtual verkot yhden virtual verkoston muodostaa yhteyden?

Maks. Perus- ja Standard dynaaminen reititys; yhdyskäytävien yhdistää 10 Suuri suorituskyvyn VPN-yhdyskäytävät 30.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Voin käyttö pisteen sivuston VPN-yhteydet Omat virtual verkoston kanssa useita VPN-tunneleita kanssa?

Kyllä-kohdassa sivuston (P2S) VPN-yhteydet voidaan käyttää VPN-yhdyskäytäviä, yhteyden muodostaminen paikalliseen sivustoissa ja muiden virtual käyttäminen.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Useita tunneleita määrittäminen virtual verkossa ja usean sivuston VPN-yhteyttä käyttämällä paikallisen oman sivuston välillä

Ei, tarpeettomat tunneleita Azure virtual verkko- ja paikallisen sivuston välillä ei tueta.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Voit siellä näkyä päällekkäin osoite välilyöntejä yhdistettyjä virtual verkkoja ja paikallisen paikallisten sivustojen välillä?

Ei. Päällekkäiset osoite välilyöntejä aiheuttavat, verkon määritysten tiedostojen lataamisen tai "Luominen Virtual Network" epäonnistuu.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Voin hankkia enemmän kuin yksi sivusto sivusto VPN-yhteydet ja yhden virtual verkoston?

Ei, kaikki VPN tunneleita, mukaan lukien pisteen sivuston VPN-yhteydet, jakaa saman Azure VPN-yhdyskäytävän ja kaistanleveyttä.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Kulkea liikenne paikalliseen Omat sivustot tai toiseen virtual verkkoon Azure VPN-yhdyskäytävän avulla?

**Perinteinen käyttöönottomalli**<br>
Salataanko siirrettävät liikenne Azure VPN-yhdyskäytävän kautta on mahdollista perinteinen käyttöönotto-mallin, mutta nimipalvelimet määritetyn osoitteen välilyöntejä verkon määritystiedostossa on riippuvainen. Erityisen ei vielä tueta Azure Virtual verkoissa ja VPN yhdyskäytävien perinteinen käyttöönotto-mallin. Erityisen, ilman määrittäminen manuaalisesti salataanko siirrettävät osoite välilyöntejä on hyvin virhe voi enää ja ei suositella.<br>
**Resurssien hallinnan käyttöönottomalli**<br>
Jos käytät resurssien hallinnan käyttöönottomalli, katso lisätietoja [erityisen](#bgp) -osasta.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Azure Luo saman IP/IKE valmiiksi jaetun avaimen kaikki omat VPN-yhteydet saman virtual verkossa?

Ei, oletusarvoisesti Azure Luo eri ennalta jaetut avaimet eri VPN-yhteydet. Voit kuitenkin määrittää laite avainarvon määrittäminen VPN yhdyskäytävän avaimen REST API tai PowerShell cmdlet-komento. Avain on oltava aakkosnumeerista merkkijono 1-128 merkin pituinen.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Azure veloittaa tietoliikenteen virtual verkkojen välillä?

Eri Azure virtual verkkojen välillä tietoliikenteen Azure kulut vain liikenne siirtyä Azure alueelta toiseen. Maksu-korko näkyy Azure [Hinnat VPN-yhdyskäytävä](https://azure.microsoft.com/pricing/details/vpn-gateway/) -sivu.


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Voinko yhdistää virtual verkon IP VPN-yhteydet, ExpressRoute-piiri?

Kyllä, tämä on tuettu. Lisätietoja on artikkelissa [määrittäminen ExpressRoute ja sivusto VPN-yhteydet, olla](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>ERITYISEN

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Paikallisen välinen yhteys- ja VMs

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Jos Omat virtuaalikoneen on virtual verkossa ja voin on paikallisen-yhteys, miten olisi voin muodostaa yhteyden AM?

Sinulla on useita vaihtoehtoja. Jos sinulla on käytössä RDP ja olet luonut päätepisteen, voit muodostaa yhteyden virtuaalikoneen VIP avulla. Siinä tapauksessa määrittää VIP ja porttia, johon haluat muodostaa yhteyden. Tarvitset virtuaalikoneen tietoliikenteen portin määrittämistä varten. Yleensä voit Siirry Azure perinteinen-portaaliin ja Tallenna RDP-yhteyden asetusten tietokoneeseen. Asetukset sisällä tarvittavat yhteystiedot.

Jos virtual verkko on määritetty paikallisen-yhteyttä, voit muodostaa yhteyden virtuaalikoneen sisäinen DIP tai yksityinen IP-osoite. Voit muodostaa yhteyden virtuaalikoneen sisäinen DIP mukaan myös toisesta virtual tietokoneesta, joka sijaitsee samassa virtual verkossa. Et voi virtuaalikoneen RDP DIP avulla, jos olet muodostamassa sijainnista virtual verkon ulkopuolella. Jos sinulla on kohdassa sivuston virtual verkko määritetty ja eivät muodosta yhteys tietokoneesta, voit esimerkiksi ei voi muodostaa yhteyden virtuaalikoneen DIP mukaan.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Jos Omat virtuaalikoneen on virtual verkon paikallisen-yhteyttä, Omat AM tietoliikenteen kerrotaan yhteyden?

Ei. Vain liikenteestä, joka on kohteen IP, jotka sisältyvät VPN paikallisen verkon IP-osoitealueet, jonka olet määrittänyt siirtyvät VPN-yhdyskäytävän kautta. Liikenne on virtual verkossa sijaitsevat IP pysyy virtual verkoston kohde. Muu tietoliikenne lähetetään Yleiset verkot kuormituksen tai pakotettu tunnelointia käytetään lähettää Azure VPN-yhdyskäytävän kautta. Jos vianmäärityksen, on tärkeää varmistaa, että sinulla on kaikki alueet, jotka on lueteltu lähiverkossa, jonka haluat lähettää yhdyskäytävän kautta. Tarkista lähiverkossa-osoitealueita mene päällekkäin osoitealueet virtual verkon avulla. Haluat myös, varmista, että käytössäsi on DNS-palvelin ratkaiseminen nimi oikea IP-osoitteeseen.


## <a name="virtual-network-faq"></a>Virtuaalinen verkon usein kysytyt kysymykset

Lisää virtual verkon tietojen tarkasteleminen [Virtual verkon usein kysytyt kysymykset](../virtual-network/virtual-networks-faq.md).
 
