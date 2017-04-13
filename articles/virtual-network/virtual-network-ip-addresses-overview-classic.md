<properties
   pageTitle="Julkiset ja yksityiset IP-osoitteiden (perinteinen) Azure-tietokannassa | Microsoft Azure"
   description="Lisätietoja julkisista ja yksityisistä IP-osoitteiden Azure-tietokannassa"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>Azure IP-osoitteet (perinteinen)
Voit määrittää IP-osoitteiden Azure resurssien pitää yhteyttä muihin Azure resursseja, paikallisen verkko ja Internet. Voit käyttää Azure IP-osoitteiden kahdenlaisia: julkisista ja yksityisistä.

Julkisten IP-osoitteiden käytetään viestintään yhteydessä Internetiin, mukaan lukien Azure julkiselle palvelut.

Yksityinen IP-osoitteiden käytetään viestintään Azure virtual verkon (VNet), pilvipalveluun ja paikallista verkkoa käytettäessä VPN-yhdyskäytävän tai ExpressRoute piiri laajentaa verkon Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Lisätietoja siitä, miten [näitä toimia käyttämällä resurssien hallinnan käyttöönottomalli](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Julkiseen IP-osoitteet
Julkisten IP-osoitteiden Salli Azure resurssien pitää yhteyttä Internet- ja Azure julkiselle palveluihin [Azure Redis välimuistin](https://azure.microsoft.com/services/cache/), [Azure tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/), [SQL-tietokannat](../sql-database/sql-database-technical-overview.md)ja [Azure-tallennustilan](../storage/storage-introduction.md).

Julkinen IP-osoite on liitetty resurssin seuraavanlaisia:

- Pilvipalveluihin
- IaaS näennäiskoneiden (VMs)
- PaaS roolin esiintymiä
- VPN yhdyskäytävät
- Sovelluksen yhdyskäytävät

### <a name="allocation-method"></a>Kohdistus-menetelmällä
Kun julkiseen IP-osoite on Azure resurssille on varattu, se on *dynaamisesti* kohdistettu resurssivarannosta käytettävissä julkinen IP-osoite luodaan resurssin sijaintia. IP-osoitteen julkaistaan, kun resurssin pysäytetään. Siltä varalta, pilvipalveluun, tämä tapahtuu, kun rooli esiintymät pysäytetään, jotka voidaan välttää *staattinen* (varattu) IP-osoitteen avulla (katso [Pilvipalveluihin](#Cloud-services)).

>[AZURE.NOTE] IP-alueita, josta Azure resurssit jaetaan julkisten IP-osoitteiden luettelo on julkaistu [Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>DNS-isäntänimi tarkkuus
Kun luot pilvipalveluun tai IaaS AM, sinun täytyy cloud palvelun DNS nimi on yksilöllinen yli Azure kaikki resurssit. Tämä luo yhdistämismäärityksen *dnsname*Azure hallita DNS-palvelimet. cloudapp.net resurssin julkiseen IP-osoitteeseen. Esimerkiksi luodessasi pilvipalveluun cloud palvelun DNS nimellä, **contoso**täydellinen toimialueen nimi (FQDN) **contoso.cloudapp.net** ratkaisee julkinen IP-osoite (VIP) cloud-palvelun. Tämä FQDN avulla voit luoda mukautetun toimialueen CNAME-tietue valitsemalla Azure julkiseen IP-osoite.

### <a name="cloud-services"></a>Pilvipalveluihin
Pilvipalveluun on aina virtual IP-osoite (VIP) kutsutaan julkiseen IP-osoite. Voit luoda päätepisteet Liitä sisäisen porttien VMs ja roolin esiintymät cloud-palvelun VIP eri portit pilvipalvelussa. 

Pilvipalvelun voi olla useita IaaS VMs tai PaaS rooli esiintymät, kaikki näkyviä saman cloud-palvelun VIP kautta. Voit myös määrittää [useita VIPs pilvipalveluun](../load-balancer/load-balancer-multivip.md), joka mahdollistaa usean VIP skenaariot, kuten usean vuokraajan-ympäristössä, jossa on SSL-pohjaiseen sivustot.

Voit varmistaa pilvipalveluun julkiseen IP-osoite säilyy ennallaan, vaikka kaikki roolin esiintymät pysäytetään *staattinen* julkisen IP-osoite, jota kutsutaan [Varattu IP](virtual-networks-reserved-public-ip.md)käyttämällä. Voit luoda staattinen IP (varattu) resurssin tietyssä paikassa ja määrittää minkä tahansa pilvipalveluun kyseisessä sijainnissa. Todellinen IP-osoitetta ei voi määrittää varattu IP-osoite, se on varattu varannon käytettävissä sijainnissa, se on luotu IP-osoitteet. Tämä IP-osoite ei ole julkaissut, kunnes poistat sen erikseen.

Staattinen (varattu) julkisten IP-osoitteiden käytetään yleensä tilanteissa, jossa pilvipalveluun:

- edellyttää on asetusten mukaan loppukäyttäjien palomuurisäännöt.
- ulkoisen DNS-nimenselvitys määräytyy ja dynaaminen IP edellyttäisi tietueiden päivittäminen.
- siinä käytetään IP-pohjainen suojausmalli käyttävien ulkoinen WWW-palvelujen.
- käyttää SSL-varmenteita linkitetty IP-osoite.

>[AZURE.NOTE] Kun luot perinteinen AM, säilön *pilvipalvelussa* luoma Azure, joka on virtual IP-osoite (VIP). Kun luominen on valmis yritysportaalin kautta, RDP oletus- tai SSH *päätepiste* on määritetty portaali niin, että voit muodostaa yhteyden AM VIP cloud-palvelun kautta. Cloud tämän palvelun VIP voidaan varata, joka sisältää muodostaa yhteyden AM varattu IP-osoite. Voit avata muita portit määrittämällä lisää päätepisteet.

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs ja PaaS rooli esiintymiä
Voit määrittää julkisen IP-osoitteen suoraan IaaS [AM](../virtual-machines/virtual-machines-linux-about.md) tai pilvipalveluun PaaS rooli esiintymä. Tätä kutsutaan nimellä esiintymän tason julkiseen IP-osoite ([ILPIP](virtual-networks-instance-level-public-ip.md)). Tämä julkiseen IP-osoite voi olla vain dynaaminen.

>[AZURE.NOTE] Tämä on erilainen kuin pilvipalvelussa, säilön IaaS VMs tai PaaS rooli esiintymät, koska pilvipalveluun voi olla useita IaaS VMs tai PaaS rooli esiintymät, kaikki saman cloud-palvelun VIP kautta tarjoamia eli VIP.

### <a name="vpn-gateways"></a>VPN yhdyskäytävät
[VPN-yhdyskäytävän](../vpn-gateway/vpn-gateway-about-vpngateways.md) avulla voidaan muodostaa yhteyden muihin Azure VNets tai paikallisen verkkojen Azure-VNet. VPN-yhdyskäytävän määritetään julkiseen IP osoite *dynaamisesti*, joka mahdollistaa viestintä remote verkoston kanssa.

### <a name="application-gateways"></a>Sovelluksen yhdyskäytävät
Azure [sovelluksen yhdyskäytävän](../application-gateway/application-gateway-introduction.md) voi käyttää Layer7-kuormituksen reitin verkkoliikennettä HTTP perusteella. Sovelluksen yhdyskäytävä on määritetty julkiseen IP osoite *dynaamisesti*, joka on kuormituksen VIP.

### <a name="at-a-glance"></a>Pikakatsaus
Alla olevassa taulukossa näkyy kunkin resurssin laji mahdollisen menetelmiä (staattinen tai dynaaminen) sekä mahdollisuus määrittää useita julkiseen IP-osoitteet.

|Resurssi|Dynaaminen|Staattinen|Useita IP-osoitteet|
|---|---|---|---|
|Pilvipalvelussa|Kyllä|Kyllä|Kyllä|
|IaaS AM tai PaaS rooli-esiintymä|Kyllä|Ei|Ei|
|VPN-yhdyskäytävän|Kyllä|Ei|Ei|
|Sovelluksen yhdyskäytävän|Kyllä|Ei|Ei|

## <a name="private-ip-addresses"></a>Yksityinen IP-osoitteet
Yksityinen IP-osoitteiden Salli Azure resurssien pitää yhteyttä muihin resursseihin pilvipalveluun tai [VPN](virtual-networks-overview.md)(VNet) tai paikalliseen verkkoon (VPN-yhdyskäytävän tai kautta ExpressRoute piiri), käyttämättä Internet tavoitettavissa IP-osoite.

Azure perinteinen käyttöönoton mallissa yksityisen IP-osoitteen voi määrittää Azure seuraavissa resursseissa:

- IaaS VMs ja PaaS rooli esiintymiä
- Sisäinen kuormituksen
- Sovelluksen yhdyskäytävän

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs ja PaaS rooli esiintymiä
Näennäiskoneiden (VMs), jotka on luotu perinteinen käyttöönottomalli sijoitetaan aina pilvipalvelussa, samalla PaaS rooli esiintymät. Yksityisten IP-osoitteiden toimintaa muistuttavat näin nämä resurssit.

On tärkeää muistaa, että pilvipalveluun käyttöön kahdella tavalla:

- Kuin *erillinen* pilvipalveluun, jos se ei ole virtual verkossa oleville.
- Osana virtual verkkoon.

#### <a name="allocation-method"></a>Kohdistus-menetelmällä
Jos kyseessä on *erillinen* pilvipalveluun resurssit Pyydä yksityinen IP varattu osoite *dynaamisesti* Azure palvelinkeskuksen yksityinen IP-osoitealueita. Sitä voi käyttää vain saman pilvipalvelussa sisällä muita VMs tietoliikenteen. IP-osoitteen voit muuttaa resurssin pysäytetään ja käytön aloittaminen.

Pilvipalveluun virtual verkon käyttöön, jos resurssit Hae yksityinen IP-osoitteet kohdistettu alueelta osoite liittyvät subnet(s) (määritettyinä sen verkon määritys). Tämä yksityinen IP-osoitteet voidaan kaikki VMs sisällä VNet väliseen viestintään.

Lisäksi VNet sisällä pilvipalveluihin, jos yksityinen IP-osoite on varattu *dynaamisesti* (joko DHCP) oletusarvoisesti. Voit muuttaa resurssin pysäytetty ja käytön aloittaminen. Jotta IP-osoite säilyy ennallaan, sinun on määritetty kohdistusmenetelmä *staattinen*ja Anna kelvollinen IP-osoite vastaavan osoitealueen sisällä.

Staattinen yksityisten IP-osoitteiden yleisesti käytetään:

 - VMs, ohjaimet toimialueen tai DNS-palvelimet.
 - VMs, jotka edellyttävät palomuurisäännöt IP-osoitteiden käyttäminen.
 - Virtuaalilaitteiksi palveluiden käyttämän muista sovelluksista IP-osoitteen kautta.

#### <a name="internal-dns-hostname-resolution"></a>Sisäinen DNS-isäntänimi tarkkuus
Kaikki Azure VMs ja PaaS rooli esiintymät on määritetty [Azure hallita DNS-palvelimet](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) oletusarvoisesti, ellet nimenomaisesti mukautettujen DNS-palvelimien määrittäminen. DNS-palvelin antaa sisäinen nimenselvitys VMs ja roolin esiintymät, jotka asuvat samassa VNet tai pilvestä palvelun.

Luodessasi AM yhdistämisessä isäntänimi yksityinen IP-osoite lisätään Azure hallita DNS-palvelimet. Jos kyseessä on usean NIC-AM isäntänimi on yhdistetty yksityinen IP-osoitteen ensisijainen NIC-sivustosta. Yhdistäminen tiedot on kuitenkin rajoitettu resurssien saman pilvipalvelussa tai VNet.

Jos kyseessä on *erillinen* pilvipalveluun osaat ratkaista isäntänimet kaikki VMs/roolin esiintymien vain saman pilvipalvelussa kuluessa. Sisällä VNet pilvipalveluun, jos osaat ratkaista isäntänimet kaikki VMs/rooli VNet sisällä esiintymät.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Sisäinen kuormituksen tasoitusmääritykset (ILB) ja sovelluksen yhdyskäytävät
Voit määrittää yksityinen IP-osoite [Azure sisäinen kuormituksen](../load-balancer/load-balancer-internal-overview.md) (ILB) tai [Azure sovelluksen yhdyskäytävän](../application-gateway/application-gateway-introduction.md) **edusta** -määritys. Tämä yksityinen IP-osoite on sisäinen päätepisteen, kaikkien käytettävissä vain resurssien sen virtual verkon (VNet) ja verkkojen yhteydessä VNet. Voit määrittää joko dynaamisen tai staattisen yksityinen IP-osoite edusta-määritys. Voit myös määrittää useita yksityisten IP-osoitteiden käyttöön usean vip skenaarioita.

### <a name="at-a-glance"></a>Pikakatsaus
Alla olevassa taulukossa näkyy kunkin resurssin laji mahdollisen menetelmiä (staattinen tai dynaaminen) sekä mahdollisuus määrittää useita yksityinen IP-osoitteet.

|Resurssi|Dynaaminen|Staattinen|Useita IP-osoitteet|
|---|---|---|---|
|AM (Valitse *erillinen* pilvipalvelussa)|Kyllä|Kyllä|Kyllä|
|PaaS roolin esiintymän (Valitse *erillinen* pilvipalvelussa)|Kyllä|Ei|Kyllä|
|AM tai PaaS roolin esiintymän (-VNet)|Kyllä|Kyllä|Kyllä|
|Sisäinen kuormituksen tasauspalvelun edusta|Kyllä|Kyllä|Kyllä|
|Sovelluksen yhdyskäytävän edusta|Kyllä|Kyllä|Kyllä|

## <a name="limits"></a>Rajoitukset

Alla olevassa taulukossa näkyy rajoissa IP-osoitteiden tilauskohtaisten Azure-tietokannassa. Voit tehdä niin, että oletusarvoinen rajoituksia ylöspäin yrityksesi tarpeiden perusteella enimmäispituus [tuelta](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

||Oletusarvoinen enimmäismäärä|Enimmäismäärä|
|---|---|---|
|Julkisten IP-osoitteiden (dynaaminen)|5|Ota yhteyttä tukeen|
|Varattu julkiseen IP-osoitteet|20|Ota yhteyttä tukeen|
|Julkinen VIP kohti käyttöönoton (pilvipalvelu)|5|Ota yhteyttä tukeen|
|Yksityinen VIP (ILB) kohti käyttöönoton (pilvipalvelu)|1|1|

Varmista, että luet käyttää kaikkia käytettävissä olevia [Verkko tiedostokokorajoituksista](azure-subscription-service-limits.md#networking-limits) Azure-tietokannassa.

## <a name="pricing"></a>Hinnat

Useimmissa tapauksissa vapaasti julkiseen IP-osoitteet. Korko.vuosi maksun käyttävät muita ja/tai staattinen julkiseen IP-osoitteita ei. Varmista, että ymmärrät [hinnat rakenne julkinen IP-osoitteet](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Resurssienhallinta ja perinteinen ominaisuuksissa väliset erot
Alla on vertailu IP lähettämisestä ominaisuuksista Resurssienhallinta ja perinteinen käyttöönottomalli.

||Resurssi|Perinteinen|Resurssien hallinta|
|---|---|---|---|
|**Julkiseen IP-osoite**|AM|Kutsutaan ILPIP (vain dynaaminen)|Kutsutaan julkiseen IP (dynaaminen tai staattinen)|
|||IaaS AM tai PaaS rooli-esiintymä|Liittyvä AM NIC: ssä|
||Internetiin yhteydessä olevan kuormituksen|Kutsutaan VIP (dynaamisen) tai varattu IP (staattinen)|Kutsutaan julkiseen IP (dynaaminen tai staattinen)|
|||Määritetty pilvipalveluun|Kuormituksen tasauspalvelun edusta config liittyvät|
||||
|**Yksityinen IP-osoite**|AM|Jäljempänä DIP|Kutsutaan yksityinen IP-osoite|
|||IaaS AM tai PaaS rooli-esiintymä|Varattu AM NIC: ssä|
||Sisäinen kuormituksen (ILB)|Liitetty ILB (dynaaminen tai staattinen)|Varattu ILB edusta määritys (dynaaminen tai staattinen)|

## <a name="next-steps"></a>Seuraavat vaiheet
- [Ota käyttöön AM staattinen yksityinen IP-osoite](virtual-networks-static-private-ip-classic-pportal.md) perinteinen-portaalissa.
