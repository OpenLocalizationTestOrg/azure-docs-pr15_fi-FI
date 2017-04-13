
<properties
   pageTitle="Sisäinen kuormituksen yleiskatsaus | Microsoft Azure"
   description="Yleistä sisäinen kuormituksen ja niiden ominaisuuksista. Kuormituksen tasauspalvelun toiminta Azure ja mahdolliset skenaarioissa sisäinen päätepisteet määrittäminen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internal-load-balancer-overview"></a>Sisäinen kuormituksen tasauspalvelun yleiskatsaus

Valitse Internetiin yhteydessä olevan kuormituksen, toisin kuin sisäinen kuormituksen (ILB) ohjaa liikenteen vain resurssit pilvipalveluun tai käyttäminen VPN Azure infrastruktuuri. Infrastruktuurin rajoittaa access kuormitus tasataan virtual IP-osoitteet (VIPs) pilvipalveluun tai Virtual Network niin, että ne koskaan olla suoraan tarkistamaan Internet-päätepisteen. Näin sisäisen rivin (LOB) yrityssovellusten Azure käynnissä ja pilveen sisältä tai resurssien paikallisen päästä.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Miksi voit joutua sisäinen kuormituksen

Azure sisäinen ladata tasaamisen (ILB) on välillä, jotka sijaitsevat pilvipalveluun tai virtual verkon aluekohtaiset laajuutta näennäiskoneiden kuormituksen. Lisätietoja virtual verkot aluekohtaiset vaikutusalueen määrittäminen ja käyttäminen kohdassa [Alueellisten Virtual verkkojen](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) Azure blogiin. Virtuaalinen verkoissa, jotka on määritetty affiniteetti ryhmän ei voi käyttää ILB.

ILB mahdollistaa kuormituksen tasaamisen seuraavanlaisia:

- Näennäiskoneiden näennäiskoneiden, jotka sijaitsevat saman pilvipalvelussa joukkoon pilvipalveluun sisällä (katso kuva 1).
- Virtuaalinen verkossa oleville näennäiskoneiden virtual verkon näennäiskoneiden, jotka sijaitsevat saman pilvipalvelussa, virtuaalisen joukkoon-verkon (katso kuva 2).
- Paikallisen-virtual verkossa paikallisen tietokoneista, jotka sijaitsevat saman pilvipalvelussa, virtuaalisen näennäiskoneiden joukkoon verkon (katso kuva 3).
- Internetiin yhteydessä oleva, monitasoisten sovellukset, jossa taustatietokantaan tasoa eivät ole Internetiin yhteydessä oleva mutta vaadi kuormituksen tietoliikenteen Internetiin yhteydessä oleva taso.
- Toimialasovellusten ylläpidettävä Azure tarvitsematta muita kuormituksen tasauspalvelun laitteiston ja ohjelmiston kuormituksen. Mukaan lukien paikallisen palvelimet tietokoneissa, joiden liikenne on kuormituksen määrittäminen saapuva.

## <a name="internet-facing-multi-tier-applications"></a>Vastakkaisten monitasoisten sovellusten Internet


Web-tason Internetiin yhteydessä olevan päätepisteet on Internet-asiakkaiden ja kuormituksen joukon osana. Kuormituksen jakaa liikenteen TCP-porttiin 443 (HTTPS) verkkosovellukset-web-palvelimiin.

Tietokannan palvelimia ILB päätepisteen joita WWW-palvelimien käyttää tallennustilan takana. Tämä tietokanta-palvelun kuormitus saapuva liikenne on saapuva ILB Määritä tietokanta-palvelinten kuormituksen päätepistettä.

Seuraava kuva esittää vastakkaisten monitasoisten sovelluksen sisällä saman pilvipalvelussa Internetissä.

Kuva 1 - vastakkaisten monitasoisten sovelluksen Internet

![Sisäinen kuormituksen yksittäisen pilvipalvelussa](./media/load-balancer-internal-overview/IC736321.png)

Monitasoisten-sovellusta voi käyttää on kun ILB käyttöön eri pilvipalveluun kuin muissa ILB palvelua näkymän.

Pilvipalveluihin käyttäminen samassa virtual verkossa on pääsy ILB päätepiste.

Kuvassa 2 on eri pilvipalvelussa tietokannan taustatietokantaan ja käyttämästä ILB päätepisteen saman virtual verkoston palvelimia edusta-WWW.

![Sisäinen kuormituksen pilvipalveluihin välillä](./media/load-balancer-internal-overview/IC744147.png)

Kuva 2 - edusta-palvelimiin eri pilvipalvelussa

## <a name="intranet-line-of-business-applications"></a>Yrityssovellusten intranet-rivi

Paikallisen verkon asiakkaiden tietoliikenteen Hae kuormituksen välillä olevat LOB palvelinten Azure verkkoon VPN-yhteyden avulla.

Asiakaskoneeseen saa käyttöoikeuden IP-osoitteen käyttäminen pisteen VPN-sivustoon Azure VPN-palvelusta. Se sallii takana ILB päätepiste LOB-sovelluksen.

![Sisäinen kuormituksen tasaamisen avulla pisteen VPN-sivustoon](./media/load-balancer-internal-overview/IC744148.png)

Kuva 3 - Toimialasovellusten kg päätepisteen takana

Toinen skenaario LOB on sivuston-sivusto VPN-yhteyden virtual verkkoon, jossa ILB päätepiste on määritetty. Näin paikallisen verkkoliikennettä reititetään ILB päätepisteelle.

![Sisäinen kuormituksen tasaamisen avulla VPN-sivuston-sivusto](./media/load-balancer-internal-overview/IC744150.png)

Kuva 4 - paikallisen verkkoliikennettä ILB päätepisteen reititetään


## <a name="next-steps"></a>Seuraavat vaiheet

[Azure kuormituksen Azure Resurssienhallinta tuki](load-balancer-arm.md)

[Aloita selainpohjainen vastakkaisten kuormituksen tasauspalvelun asetusten määrittäminen](load-balancer-get-started-internet-arm-ps.md)

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-ps.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)

