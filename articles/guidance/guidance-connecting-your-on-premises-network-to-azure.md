<properties
   pageTitle="Paikallisen verkon muodostamisesta Azure | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan ja vertaa eri tavat käytettävissä muodostettaessa yhteyttä Microsoftin pilvipalveluihin, kuten Azure, Office 365: ssä ja Dynamics CRM Online."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Yhteyden muodostaminen Azure paikalliseen verkkoon

Microsoft tarjoaa erilaisia pilvipalveluihin. Kun pystyt muodostamaan yhteyden kaikki palvelut julkisen Internetin välityksellä, voit myös muodostaa joitakin käyttämällä näennäisen yksityisverkon (VPN) tunnelin Internetin kautta tai suorassa yhteydessä Microsoftille palveluiden. Tämän artikkelin avulla voit selvittää, mitkä yhteystapa parhaiten vastaa tarpeitasi, Microsoftin pilvipalveluihin, voit käyttää tyypit perusteella. Useimmat organisaatiot käyttämiseen useita lajit seuraavalla tavalla.

## <a name="connecting-over-the-public-internet"></a>Yhteyden muodostaminen julkisesta Internetistä

Tämä yhteystyyppi tarjoaa pääsyn Microsoftin pilvipalveluihin suoraan Internetin välityksellä, alla kuvatulla tavalla.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Tässä yhteydessä on yleensä ensimmäinen tyyppi, jota käytetään muodostettaessa yhteyttä Microsoftin pilvipalveluihin. Seuraavassa taulukossa luetellaan etuja sekä yhteyden tällaista haittoja.



| **Edut**| **Huomioon otettavia seikkoja**|
|---------|---------|
|Edellyttää mitään muutoksia paikalliseen verkkoon, kunhan kaikki asiakaslaitteet on oikeus kaikki IP-osoitteista ja porteista Internetissä.|Liikenne on usein salattu HTTPS-yhteyden avulla, vaikka se voivat kaapata siirrettäessä, koska se traverses julkinen Internet.|
|Voit muodostaa yhteyden kaikkiin julkinen Internet-yhteyttä Microsoftin pilvipalveluihin.|Odottamattomat viive koska traverses yhteys Internetiin.|
|Käyttää Internet-yhteys.||
|Yhteyksien laitteiden hallinta ei tarvita.||

Tätä yhteyttä ei ole connectivity tai kaistanleveyden kustannukset jälkeen, kun käytät Internet-yhteys. 

## <a name="connecting-with-a-point-to-site-connection"></a>Pisteen sivuston yhteys muodostetaan

Tämä yhteystyyppi tarjoaa pääsyn jotkin Microsoftin pilvipalveluihin Secure Socket Tunneling Protocol (SSTP) tunnelilla Internetin kautta, alla kuvatulla tavalla.

![P2S] (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "Pisteen sivuston yhteyden")

Yhteys tehdään aiemmin Internet-yhteyden kautta, mutta vaatii Azure VPN-yhdyskäytävän. Seuraavassa taulukossa luetellaan etuja sekä yhteyden tällaista haittoja.

| **Edut**| **Huomioon otettavia seikkoja**|
|---------|---------|
|Edellyttää mitään muutoksia paikalliseen verkkoon, kunhan kaikki asiakaslaitteet on oikeus kaikki IP-osoitteista ja porteista Internetissä.|Vaikka liikenne on salattu IP-se voivat kaapata siirrettäessä, koska se traverses julkinen Internet.|
|Käyttää Internet-yhteys.|Odottamattomat viive koska traverses yhteys Internetiin.|
|Siirtonopeuden enintään 200 Mt/s yhdyskäytävän kohden.|Edellyttää luomisen ja hallinnan eri paikallisen verkon kunkin laitteen ja kunkin yhdyskäytävän muodostamiseen tarvittavat kunkin laitteen väliset yhteydet.|
|Voidaan muodostaa Azure-palveluihin, joiden yhdistämistä tuetaan Azure Virtual verkkoihin (VNet) kuten Azuren näennäiskoneiden ja Azure pilvipalveluihin.|Edellyttää mahdollisimman vähän Azure VPN-yhdyskäytävän jatkuvaa hallinta.|
||Ei voi käyttää Microsoft Office 365-tai Dynamics CRM Online.
||Ei voi muodostaa Azure palvelut, joita ei voi yhdistää VNet.|

Lisätietoja [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) -palveluun, sen [hinnat](https://azure.microsoft.com/pricing/details/vpn-gateway)ja lähtevien tietojen siirto [hinnat](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Yhteyden muodostaminen sivusto sivusto-yhteydessä

Tämä yhteystyyppi pääsee joitakin Microsoftin pilvipalveluihin kautta IP-suojaustunnelin Internetin kautta, alla kuvatulla tavalla.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Sivusto yhteys")

Yhteys tehdään aiemmin Internet-yhteyden kautta, mutta vaatii Azure VPN-yhdyskäytävän liittyvät hinnat ja lähtevien tietojen siirto hinnat. Seuraavassa taulukossa luetellaan etuja sekä yhteyden tällaista haittoja.

| **Edut**| **Huomioon otettavia seikkoja**|
|---------|---------|
|Kaikkien laitteiden paikallisen verkon viestiä Azure-palvelujen yhteydessä VNet niin ei tarvitse määrittää yksittäisen yhteydet kunkin laitteen kanssa.|Vaikka liikenne on salattu IP-se voivat kaapata siirrettäessä, koska se traverses julkinen Internet.|
|Käyttää Internet-yhteys.|Odottamattomat viive koska traverses yhteys Internetiin.|
|Voidaan muodostaa yhteyden Azure palvelut, joiden yhdistämistä tuetaan VNet, kuten näennäiskoneiden ja pilvipalveluihin.|Täytyy määrittää ja hallita vahvistettu VPN laitteen * paikallisen.|
|Siirtonopeuden enintään 200 Mt/s yhdyskäytävän kohden.|Edellyttää mahdollisimman vähän Azure VPN-yhdyskäytävän jatkuvaa hallinta.|
|Voit pakottaa lähtevän tietoliikenteen cloud näennäiskoneiden tarkastuksen ja käyttäjän määrittämät tiet tai reunan yhdyskäytävän Protocol (erityisen) kirjaaminen paikallisen verkon kautta käynnistetty **.|Ei voi käyttää Microsoft Office 365-tai Dynamics CRM Online.|
||Ei voi muodostaa Azure palvelut, joita ei voi yhdistää VNet.|
||Jos käytät palveluja, jotka aloittavat yhteydet takaisin paikalliseen laitteet ja suojaus-käytännöt edellyttävät sitä, voit joutua palomuuri paikalliseen verkkoon ja Azure välillä.|

- * Tarkastella luetteloa [vahvistettu VPN-laitteet](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices).
- ** Lisätietoja pakottaa Azure VNets reitittämisestä paikallisen laitteen [käyttäjän määrittämä tiet](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) tai [erityisen](../vpn-gateway/vpn-gateway-bgp-overview.md) avulla.

## <a name="connecting-with-a-dedicated-private-connection"></a>Oma yksityinen yhteys muodostetaan

Tämän tietoyhteyden avulla voit käyttää kaikkien Microsoftin pilvipalveluihin oma yksityinen-yhteyden Microsoftille, joka ei käy läpi Internet-alla kuvatulla tavalla.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute yhteys")

Yhteys edellyttää ExpressRoute-palvelu ja yhteyden connectivity-palvelun käyttöä. Seuraavassa taulukossa luetellaan etuja sekä yhteyden tällaista haittoja.

| **Edut**| **Huomioon otettavia seikkoja**|
|---------|---------|
|Liikenne ei voi kaapata siirron julkisen Internetin jälkeen palveluntarjoajan erillinen yhteys käytetään.|Edellyttää paikallisen reitittimen hallinta.|
|Kaistanleveys enintään 10 gt/s ExpressRoute piiri ja siirtonopeuden enintään 2 gt/s kunkin yhdyskäytävän kohden.|Edellyttää erillinen yhteyden connectivity-palvelun.|
|Ennakoitavissa viive koska se on erillinen Microsoft, joka ei käy läpi Internet-yhteyden.|Saattaa edellyttää vähintään yksi Azure VPN yhdyskäytävää mahdollisimman vähän jatkuvaa hallinta (Jos yhteyden virtapiirin VNets).|
|Ei edellytä salattu tietoliikenne, mutta voit voit salata liikenne.| Jos käytät pilvipalveluihin, Aloita yhteydet takaisin paikalliseen laitteet, joudut ehkä palomuuri paikalliseen verkkoon ja Azure välillä.|
|Suoraan muodostaa yhteyden Microsoft cloud-palvelun, muutaman poikkeukset *.|Verkko-osoitteiden muuntaminen (NAT) edellyttää paikallisen IP-osoitteiden syöttäminen palvelut, joita ei voi yhdistää VNet.* * Microsoft tietojen keskikohdan mukaan|
|Voit pakottaa lähtevän tietoliikenteen käynnistetty cloud näennäiskoneiden tarkastuksen ja kirjaamisen käyttäen erityisen paikallisen verkon kautta.|

- * Tarkastella [palveluluetteloon](../expressroute/expressroute-faqs.md#supported-services) ei voi käyttää ExpressRoute kanssa. Azure-tilauksesi on hyväksyttävä muodostaa yhteyden Office 365: ssä.  Lisätietoja artikkelista [Azure ExpressRoute Office 365: n](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) .
- ** Lisätietoja ExpressRoute [NAT](../expressroute/expressroute-nat.md) vaatimuksia.

Lisätietoja [ExpressRoute](../expressroute/expressroute-introduction.md)sen liittyvät [hinnat](https://azure.microsoft.com/pricing/details/expressroute)ja [yhdistämispalvelua tarjoajat](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Muita huomioon otettavia seikkoja

- Useita yllä on eri suurin rajoitukset, niiden tukemista VNet yhteydet, yhdyskäytävän yhteydet ja muun kriteerin perusteella. On suositeltavaa, että luet Azure [Verkko rajoittaa](../azure-subscription-service-limits.md#networking-limits) selvittääksesi, jos halutut arvosarjat vaikuttaa yhteyden tyypit, voit valita, haluatko käyttää. 
- Jos aiot yhdyskäytävän yhdistäminen saman VNet kuin ExpressRoute yhdyskäytävän sivusto sivusto VPN-yhteyden, sinun olisi valintanauhaan tutustuminen tärkeitä rajoitukset ensin. Lisätietoja [määrittäminen ExpressRoute ja sivuston sivuston samanaikaisesti olemassa olevien yhteydet](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) -artikkelissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Resurssit alla kerrotaan ottamisesta käyttöön tämän artikkelin lajit.

-   [Ota käyttöön kohta sivuston yhteyden](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Sivusto yhteyden käyttöönotto](guidance-hybrid-network-vpn.md)
-   [Ota käyttöön erillinen yksityisen yhteyden](guidance-hybrid-network-expressroute.md)
-   [Ottaa käyttöön erillinen yksityinen yhteys, jonka sivuston sivuston yhteyden suuri käytettävyys](guidance-hybrid-network-expressroute-vpn-failover.md)
