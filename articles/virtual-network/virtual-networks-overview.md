<properties
   pageTitle="Azure Virtual Network (VNet) yleiskatsaus"
   description="Lisätietoja Azure virtual verkot (VNets)."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="virtual-network-overview"></a>VPN-yleiskatsaus

Azure virtual verkon (VNet) on omat verkon pilveen esitys.  On looginen eristystaso Azure pilven erillinen tilaukseen. Voit hallita täysin esimerkiksi IP-osoitelohkot DNS asetukset, suojauskäytäntöjä ja reitin verkoston taulukoista. Voit myös muita määritetään oman VNet aliverkosta kyselyjä ja Käynnistä Azure IaaS näennäiskoneiden (VMs) ja/tai [pilvipalveluihin (PaaS rooli esiintymät)](../cloud-services/cloud-services-choose-me.md). Lisäksi voit muodostaa virtual verkon jollakin [yhteysasetukset](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) käytettävissä Azure paikalliseen verkkoon. Oikeastaan voit laajentaa verkon Azure, ja IP-osoite kuutiot, joissa on yrityksen asteikko Azure on hyötyä valmis ohjausobjektiin.

Ymmärtämään VNets, tutustu kuvan alla, jossa näkyy yksinkertaistettu paikalliseen verkkoon.

![Paikalliseen verkkoon](./media/virtual-networks-overview/figure01.png)

Edellä olevassa kuvassa reitittimen kautta julkinen Internet-yhteys paikalliseen-verkkoon. Näet myös sekä DMZ, isännöinnin DNS-palvelin ja verkko-palvelinfarmiin palomuuri. Web-palvelinfarmin on kuormitus tasataan laitteiston kuormituksen, joka on määritetty yhteyden Internetiin, ja siinä käytetään resursseja sisäinen aliverkosta avulla. Sisäinen aliverkon erotetaan DMZ toisen palomuurin ja isännät Active Directory-toimialueen ohjauskoneen palvelimet, tietokannan palvelimia ja sovelluksen palvelimiin.

Samassa verkossa voidaan isännöidä Azure alla olevassa kuvassa osoitetulla tavalla.

![Azure virtual verkossa](./media/virtual-networks-overview/figure02.png)

Huomaa, miten Azure infrastruktuurin kestää rooli reitittimen sallitaan-että VNet ilman, että minkä tahansa kokoonpanon julkinen Internet-yhteys. Palomuurit voivat korvataan verkko-käyttöoikeusryhmät (NSGs) käytetään kunkin yksittäisen aliverkon. Ja fyysinen kuormituksen tasoitusmääritykset ovat korvataan internet aukeaman ja sisäisistä kuormituksen tasoitusmääritykset Azure-tietokannassa.

>[AZURE.NOTE] Azure-tietokannassa on kaksi käyttöönoton tilaa: perinteinen (tunnetaan myös nimellä Service Management) ja Azure resurssien hallinta (ARM). Perinteinen VNets voitu affiniteetti ryhmään tai alueellisen VNet luodaan. Jos sinulla VNet affiniteetti-ryhmässä, on suositeltavaa siirtää [sen aluekohtaiset VNet](virtual-networks-migrate-to-regional-vnet.md).

## <a name="virtual-network-benefits"></a>VPN-edut

- **Yksinään**. VNets eristetään kokonaan toisistaan. Jonka avulla voit luoda erillisten kehitystä, testaus ja tuotannon verkkojen, jotka käyttävät samaa CIDR osoitelohkot.

- **Julkinen Internet-yhteys**. Rooli esiintymät kaikki IaaS VMs ja PaaS VNet käyttää julkinen Internet oletusarvoisesti. Verkon käyttöoikeusryhmät (NSGs) avulla voidaan hallita.

- **Access VNet sisällä VMs**. PaaS roolin esiintymät ja IaaS VMs voidaan käynnistää virtual samassa verkostossa ja he voivat muodostaa toisiinsa käyttämällä yksityisten IP-osoitteiden, vaikka heillä ei olisikaan eri aliverkosta ilman, että yhdyskäytävän määrittämiseen tai käytä julkiseen IP-osoitteita.

- **Nimenselvitys**. Azure tarjoaa sisäinen nimenselvitys IaaS VMs ja otettu käyttöön oman VNet PaaS rooli esiintymät. Voit ottaa käyttöön oman DNS-palvelimet ja määrittäminen käyttämään niitä VNet.

- **Suojaus**. Kirjoittaminen ja sulkemisen näennäiskoneiden ja PaaS rooli esiintymät VNet-liikenne voidaan hallita verkon käyttöoikeusryhmät.

- **Yhteys**. VNets on yhdistetty toisiinsa verkon yhdyskäytävien tai VNet peering avulla. VNets voidaan yhdistää paikallisen tietojen centersin sivusto sivusto VPN-verkoissa tai Azure ExpressRoute kautta. Lisätietoja sivuston sivuston VPN-yhteys, käy [tietoja VPN yhdyskäytävät](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Saat lisätietoja ExpressRoute seuraavasta [ExpressRoute teknisiä tietoja](../expressroute/expressroute-introduction.md). Lisätietoja VNet peering käy [VNet peering](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Varmista, että luot VNet ennen kuin otat IaaS VMs- tai PaaS rooli esiintymät Azure-ympäristöön. ARM-pohjainen VMs edellyttävät VNet ja jos et määritä aiemmin VNet, Azure luo oletusarvoisesti VNet, joilla CIDR osoite estä olla paikallisen verkoston kanssa. Tekeminen et voi muodostaa yhteyttä VNet paikalliseen verkkoon.

## <a name="subnets"></a>Aliverkosta

Aliverkon on alueen IP-osoitteiden-VNet, VNet voidaan jakaa useisiin aliverkosta organisaation ja suojaus. VMs ja PaaS rooli esiintymät käyttöön aliverkosta (samassa tai eri) sisällä VNet voi olla yhteydessä toisiinsa ilman ylimääräisiä kokoonpanoa. Voit myös määrittää reititystaulukot ja NSGs aliverkon.

## <a name="ip-addresses"></a>IP-osoitteet


IP-osoitteiden varattujen resurssien Azure kahdenlaisia: *julkisista* ja *yksityisistä*. Julkisten IP-osoitteiden Salli Azure resurssien pitää yhteyttä Internet- ja muut Azure julkiselle-palveluja, kuten [Azure Redis välimuisti](https://azure.microsoft.com/services/cache/)- [Azure tapahtuman keskittimet](https://azure.microsoft.com/documentation/services/event-hubs/). Yksityinen IP-osoitteiden avulla välisen resurssien virtual verkossa, ja ne kautta VPN-käyttämättä Internet reititettävä IP-osoitteet.

Lisätietoja Azure IP-osoitteet, käy [VPN IP-osoitteet](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Azure kuormituksen tasoitusmääritykset

Näennäiskoneiden ja pilvipalveluihin Virtual verkossa, voit nähdä Internetissä Azure kuormituksen tasoitusmääritykset. Sisäisten olevien yrityssovellusten voi olla vain kuormitus tasataan sisäinen kuormituksen avulla.

- **Ulkoinen kuormituksen**. Voit käyttää ulkoisen kuormituksen antamaan suuren käytettävyyden IaaS VMs ja PaaS rooli esiintymien käyttää julkisesta Internetistä.

- **Sisäinen kuormituksen**. Voit säätää suuren käytettävyyden IaaS VMs ja PaaS rooli esiintymät muiden oman VNet pääsee sisäinen kuormituksen.

Lisätietoja kuormituksen Azure-tietokannassa, käy [ladata tasaustoiminto yleiskatsaus](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Verkko-käyttöoikeusryhmän (NSG)

Voit luoda NSGs hallintaa saapuvan ja lähtevän verkkoliittymät (NIC) VMs ja aliverkosta. Kunkin NSG on yksi tai määrittäminen, onko liikenne on hyväksytty tai estetty sääntöjen perusteella lähde-IP-osoite, Lähdeportti, kohde-IP-osoite ja kohdeportti. Saat lisätietoja NSGs seuraavasta [verkon käyttöoikeusryhmän ominaisuudet](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Virtuaalinen laitteiden

Virtuaalinen laitteen on vain toisen AM-että VNet, joka suoritetaan ohjelmiston mukaan laitteen-funktiota, kuten palomuurin tai WAN optimointi tunkeutumisen tunnistus. Voit luoda reitin Azure reitittämään VNet liikenne kautta virtual laitteen, voit käyttää sen ominaisuuksia.

Esimerkiksi NSGs voidaan antaa oman VNet suojaus. NSGs voi kuitenkin säätää kerroksen 4 Käyttöoikeusluettelon Access Control List () Saapuneet ja lähetetyt-pakettien. Jos haluat käyttää kerroksen 7 suojausmalli, sinun on käytettävä palomuuri-laitteen.

Virtuaalinen laitteiden määräytyvät sen mukaan, [käyttäjän määrittämä tiet ja IP-välityksen](virtual-networks-udr-overview.md).

## <a name="limits"></a>Rajoitukset
On sallittu tilauksen Virtual verkkojen enimmäismäärään, lue lisätietoja [Azure verkko rajoitukset](../azure-subscription-service-limits.md#networking-limits) lisätietoja.

## <a name="pricing"></a>Hinnat
Ei ole ylimääräisiä kustannuksia Azure Virtual verkkojen käyttämistä varten. Laske esiintymät käynnistetty sisällä Vnet veloitetaan normaalin korvauksen kuvatulla tavalla [Azure AM hinnat](https://azure.microsoft.com/pricing/details/virtual-machines/). [VPN-yhdyskäytäviä](https://azure.microsoft.com/pricing/details/vpn-gateway/) ja [julkisten IP-osoitteiden] (https://azure.microsoft.com/pricing/details/ip-addresses/) VNet käytetään myös on ladattu normaalin korvauksen.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Luo VNet](virtual-networks-create-vnet-arm-pportal.md) ja aliverkosta.
- [Luo-VNet AM](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Lisätietoja [NSGs](virtual-networks-nsg.md).
- Lisätietoja [käyttäjän määrittämät tiet ja IP-välityksen](virtual-networks-udr-overview.md).
