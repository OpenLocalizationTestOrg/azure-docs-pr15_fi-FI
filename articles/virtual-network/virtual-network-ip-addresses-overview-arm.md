<properties
   pageTitle="Julkiset ja yksityiset IP-osoitteiden Azure Resurssienhallinta | Microsoft Azure"
   description="Lisätietoja julkisista ja yksityisistä IP-osoitteiden Azure resurssien hallinta"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>Azure IP-osoitteet
Voit määrittää IP-osoitteiden Azure resurssien pitää yhteyttä muihin Azure resursseja, paikallisen verkko ja Internet. Voit käyttää Azure IP-osoitteiden kahdenlaisia:

- **Julkiseen IP-osoitteet**: käytetään viestintään yhteydessä Internetiin, mukaan lukien Azure julkiselle palvelut
- **Yksityinen IP-osoitteet**: käytetään viestintään Azure virtual verkon (VNet) ja paikallisen oman verkon käytettäessä VPN-yhdyskäytävän tai ExpressRoute piiri laajentaa verkon Azure.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönottomalli](virtual-network-ip-addresses-overview-classic.md).

Jos olet tutustunut perinteinen käyttöönotto-mallin, tarkista [IP-osoitteiden välillä perinteinen erot ja Resurssienhallinta](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Julkiseen IP-osoitteet
Julkisten IP-osoitteiden Salli Azure resurssien pitää yhteyttä Internet- ja Azure julkiselle palveluihin [Azure Redis välimuistin](https://azure.microsoft.com/services/cache/), [Azure tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/), [SQL-tietokannat](../sql-database/sql-database-technical-overview.md)ja [Azure-tallennustilan](../storage/storage-introduction.md).

Azure resurssien hallinnan [julkiseen IP](resource-groups-networking.md#public-ip-address) -osoite on resurssi, joka on omia ominaisuuksia. Voit yhdistää julkiseen IP-osoite resurssin jokin on seuraavissa resursseissa:

- Näennäiskoneiden (AM)
- Internetiin yhteydessä oleva kuormituksen tasoitusmääritykset
- VPN yhdyskäytävät
- Sovelluksen yhdyskäytävät

### <a name="allocation-method"></a>Kohdistus-menetelmällä
Jossa IP-osoite on varattu resurssitaulukkoon *julkiseen* IP - *dynaamisen* tai *staattisen*kahdella tavalla. Kohdistus oletustapa on *dynaaminen*, jos IP-osoite **on varattu sen luomisen yhteydessä** . Julkinen IP-osoite on varattu, kun käynnistät (tai luoda) liittyvän resurssi (kuten AM tai kuormituksen tasauspalvelun). IP-osoite julkaistaan, kun haluat lopettaa (tai poistaa) resurssi. Tämä aiheuttaa muuttuvat, kun Lopeta ja Käynnistä resurssin IP-osoite.

Jotta liittyvän resurssin IP-osoite säilyy ennallaan, voit määrittää kohdistusmenetelmä eksplisiittisesti *staattiseksi*. Tässä tapauksessa IP-osoite on määritetty heti. Se julkaistaan vain, kun poistaminen resurssin tai varauksen sen laskentatavan muuttaminen *dynaamiseksi*.

>[AZURE.NOTE] Myös silloin, kun määrität kohdistusmenetelmä *staattinen*, et voi määrittää *julkiseen IP-resurssien*toteutunut IP-osoite. Sen sijaan se saa kohdistettu varannon käytettävissä IP-osoitteiden resurssin luodaan Azure sijainnissa.

Staattinen julkisten IP-osoitteiden yleisesti luodaan seuraavissa tilanteissa:

- Loppukäyttäjien on päivitettävä palomuurisäännöt Azure resurssien yhteydessä.
- DNS-nimenselvitys, jossa IP-osoite muutoksen edellyttäisi tietueiden päivittäminen.
- Azure resurssien yhteydessä muiden sovellusten tai palveluihin, jotka käyttävät IP-osoite perustuva suojaus-malli.
- Voit käyttää SSL-varmenteita linkitetty IP-osoite.

>[AZURE.NOTE] IP-alueita, josta julkiseen IP-osoitteet (staattinen tai dynaaminen) on kohdistettu Azure resursseihin luettelo on julkaistu [Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>DNS-isäntänimi tarkkuus
Voit määrittää DNS toimialueen nimi tarran julkiseen IP-resurssi, joka luo *domainnamelabel*yhdistämismääritys. *sijainnin*. cloudapp.azure.com Azure hallita DNS-palvelimet julkiseen IP-osoite. Esimerkiksi jos luot julkiseen IP-resurssin **contoso** kuin *domainnamelabel* **Länsi US** Azure *sijainti*-täydellinen toimialueen nimi (FQDN) **contoso.westus.cloudapp.azure.com** ratkaisee resurssin julkiseen IP-osoitteeseen. Tämä FQDN avulla voit luoda mukautetun toimialueen CNAME-tietue valitsemalla Azure julkiseen IP-osoite.

>[AZURE.IMPORTANT] Kunkin toimialueen nimi tarran luotu on oltava yksilöllinen Azure paikkaan.  

### <a name="virtual-machines"></a>Näennäiskoneiden
Voit liittää julkiseen IP-osoite [Windows](../virtual-machines/virtual-machines-windows-about.md) - tai [Linux](../virtual-machines/virtual-machines-linux-about.md) AM määrittämällä **verkkoliittymän**. Usean verkon-liittymän AM kyseessä määrittää sen *ensisijaisen* verkkoliittymän vain. Voit määrittää dynaamisen tai staattinen julkiseen IP-osoite AM.

### <a name="internet-facing-load-balancers"></a>Internetiin yhteydessä oleva kuormituksen tasoitusmääritykset
Voit yhdistää julkiseen IP-osoite [Azure ladata tasaustoiminto](../load-balancer/load-balancer-overview.md)määrittämällä kuormituksen tasauspalvelun **edusta** -määritys. Tämä julkiseen IP-osoite on jokin kuormituksen virtual IP-osoite (VIP). Voit määrittää dynaamisen tai staattinen julkiseen IP-osoite edusta kuormituksen. Voit myös liittää useita julkiseen IP-osoitteita kuormituksen edusta, joka mahdollistaa [Usean VIP](../load-balancer/load-balancer-multivip.md) skenaariot, kuten usean vuokraajan-ympäristössä, jossa on SSL-pohjaiseen sivustot.

### <a name="vpn-gateways"></a>VPN yhdyskäytävät
[Azure VPN-yhdyskäytävän](../vpn-gateway/vpn-gateway-about-vpngateways.md) avulla luodaan yhteys Azure virtual verkon (VNet) muiden Azure VNets tai paikalliseen verkkoon. Sinun täytyy määrittää julkisen IP-osoitteen, jotta se pitää yhteyttä remote verkon sen **IP-määritys** . Tällä hetkellä vain määrittää *dynaamisen* julkiseen IP-osoite VPN-yhdyskäytävään.

### <a name="application-gateways"></a>Sovelluksen yhdyskäytävät
Voit yhdistää julkiseen IP-osoite Azure [Sovelluksen yhdyskäytävän](../application-gateway/application-gateway-introduction.md)määrittämällä yhdyskäytävän **edusta** -määritys. Tämä julkiseen IP-osoite on kuormituksen VIP. Tällä hetkellä voit lisätä *dynaamisen* julkiseen IP-osoite sovelluksen yhdyskäytävän edusta-määrityksiin.

### <a name="at-a-glance"></a>Osoitteessa--vilkaisulla
Seuraavassa taulukossa esitetään tietyn ominaisuuden, jonka kautta julkisen IP-osoitteen voi liittää ylimmän tason resurssi ja mahdollisen menetelmiä (dynaaminen tai staattinen), jonka avulla voidaan.

|Ylimmän tason resurssi|IP-osoite liitos|Dynaaminen|Staattinen|
|---|---|---|---|
|Virtuaalikoneen|Verkkokäyttöliittymä|Kyllä|Kyllä|
|Kuormituksen|Edusta-määritys|Kyllä|Kyllä|
|VPN-yhdyskäytävän|Yhdyskäytävän IP-määritys|Kyllä|Ei|
|Sovelluksen yhdyskäytävän|Edusta-määritys|Kyllä|Ei|

## <a name="private-ip-addresses"></a>Yksityinen IP-osoitteet
Yksityinen IP-osoitteiden Salli Azure resurssien yhteydessä [VPN](virtual-networks-overview.md) -tai paikallisen-verkon kautta VPN-yhdyskäytävän tai ExpressRoute piiri muita resursseja ilman Internet-tavoitettavissa IP-osoitetta käyttämällä.

Azure resurssien hallinnan käyttöönotto-mallissa yksityinen IP-osoite on liitetty Azure resurssien seuraavanlaisia:

- VMs
- Sisäinen kuormituksen tasoitusmääritykset (ILBs)
- Sovelluksen yhdyskäytävät

### <a name="allocation-method"></a>Kohdistus-menetelmällä
Yksityinen IP-osoite on varattu, johon resurssi kuuluu aliverkon osoite-alueelta. Itse aliverkon osoitealue on osa VNet osoitealueita.

Jossa yksityinen IP-osoite on varattu kahdella tavalla: *dynaamisen* tai *staattisen*. Kohdistus oletustapa on *dynaaminen*, jossa IP-osoitteen automaattisesti kohdistettu resurssin aliverkosta (joko DHCP). Kun Lopeta ja Käynnistä resurssin muuttaa IP-osoitteen.

Voit määrittää kohdistusmenetelmä *staattinen* IP-osoite pysyy sama. Tässä tapauksessa myös haluat antaa kelvollinen IP-osoite, joka on resurssin aliverkon osa.

Staattinen yksityisten IP-osoitteiden yleisesti käytetään:

- VMs, ohjaimet toimialueen tai DNS-palvelimet.
- Palomuurisäännöt IP-osoitteiden käyttäminen vaativien resurssien.
- Käytä IP-osoitteen kautta muut sovellukset ja resurssit resursseja.

### <a name="virtual-machines"></a>Näennäiskoneiden
Yksityinen IP-osoite on määritetty [Windows](../virtual-machines/virtual-machines-windows-about.md) -tai [Linux](../virtual-machines/virtual-machines-linux-about.md) AM **verkkoliittymän** . Jos kyseessä on useita verkko-liittymän AM kunkin liittymän saa määritetty yksityinen IP-osoite. Voit määrittää dynaaminen tai staattinen verkko-liittymän kohdistusmenetelmää.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Sisäinen DNS hostname tarkkuuden (VMs)
Kaikki Azure VMs määritetään [Azure hallita DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) -palvelinten oletusarvoisesti, ellet nimenomaisesti mukautettujen DNS-palvelimien määrittäminen. DNS-palvelin antaa sisäinen nimenselvitys VMs, jotka sijaitsevat saman VNet.

Luodessasi AM yhdistämisessä isäntänimi yksityinen IP-osoite lisätään Azure hallita DNS-palvelimet. Jos kyseessä on useita verkko-liittymän AM isäntänimi on nyt yhdistetty ensisijainen verkkoliittymän yksityinen IP-osoite.

Määritetty Azure hallita DNS-palvelimet VMs voi ratkaista kaikkien niiden VNet sisällä VMs isäntänimet niiden yksityinen IP-osoitteisiin.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Sisäinen kuormituksen tasoitusmääritykset (ILB) ja sovelluksen yhdyskäytävät
Voit määrittää yksityinen IP-osoite [Azure sisäinen kuormituksen](../load-balancer/load-balancer-internal-overview.md) (ILB) tai [Azure sovelluksen yhdyskäytävän](../application-gateway/application-gateway-introduction.md) **edusta** -määritys. Tämä yksityinen IP-osoite on sisäinen päätepisteen, kaikkien käytettävissä vain resurssien sen virtual verkon (VNet) ja verkkojen yhteydessä VNet. Voit määrittää joko dynaamisen tai staattisen yksityinen IP-osoite edusta-määritys.

### <a name="at-a-glance"></a>Osoitteessa--vilkaisulla
Seuraavassa taulukossa esitetään tietyn ominaisuuden, jonka kautta yksityisen IP-osoitteen voi liittää ylimmän tason resurssi ja mahdollisen menetelmiä (dynaaminen tai staattinen), jonka avulla voidaan.

|Ylimmän tason resurssi|IP-osoite liitos|Dynaaminen|Staattinen|
|---|---|---|---|
|Virtuaalikoneen|Verkkokäyttöliittymä|Kyllä|Kyllä|
|Kuormituksen|Edusta-määritys|Kyllä|Kyllä|
|Sovelluksen yhdyskäytävän|Edusta-määritys|Kyllä|Kyllä|

## <a name="limits"></a>Rajoitukset

IP-osoitteiden asetettuja rajoituksia on esitetty käyttää kaikkia käytettävissä olevia [Verkko tiedostokokorajoituksista](azure-subscription-service-limits.md#networking-limits) Azure-tietokannassa. Nämä raja-arvot ovat alueittain ja tilauskohtaisten. Voit tehdä niin, että oletusarvoinen rajoituksia ylöspäin yrityksesi tarpeiden perusteella enimmäispituus [tuelta](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="pricing"></a>Hinnat

Julkisten IP-osoitteiden on ehkä korko.vuosi maksu. Lisätietoja IP-osoite hinnat Azure-tietokannassa, tarkista [IP-osoite hinnoittelu](https://azure.microsoft.com/pricing/details/ip-addresses) -sivulta.

## <a name="next-steps"></a>Seuraavat vaiheet
- [Ottaa käyttöön AM kanssa staattinen julkiseen IP Azure-portaalissa](virtual-network-deploy-static-pip-arm-portal.md)
- [Ottaa käyttöön AM kanssa staattinen julkiseen IP mallin avulla](virtual-network-deploy-static-pip-arm-template.md)
- [Staattinen yksityisen IP-osoitteen Azure-portaalissa AM käyttöönotto](virtual-networks-static-private-ip-arm-pportal.md)
