
<properties
   pageTitle="Azure ohjeet | kuvioiden ja käytännöt | Microsoft Azure"
   description="Azure viittaus arkkitehtuureihin"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure viittaus arkkitehtuureihin

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Tämä sisältö on aktiivinen kehittäminen. Se on hyödyllinen tänään, jotta Microsoft tekee sen käytettävissä esikatselu. Arvostamme antamaasi palautetta._

Tutustu viittaus arkkitehtuureihin asettelu tilanteessa, jossa useita liittyvät arkkitehtuureihin ryhmiteltynä.
Kunkin yksittäisiä arkkitehtuuri tarjoaa suositeltuja käytännöt ja saat vaiheet sekä suoritettavan tiedoston osa, joka embodies suositukset.
Monet arkkitehtuureihin ovat asteittain; rakentaminen edellisen arkkitehtuureihin, joka on vähemmän vaatimuksia päälle.

## <a name="designing-your-infrastructure-for-resiliency"></a>Infrastruktuurin vikasietoisuudelle suunnitteleminen

Tämä sarja parhaimman AM määritysten suositeltuja käytännöt alkava ja culminates monille-ympäristö, jonka automaattisesti.

- [Käynnissä AM Windows Azure](guidance-compute-single-vm.md)
- [Linux AM käytössä Azure](guidance-compute-single-vm-linux.md)
- [Käynnissä useita VMs skaalattavuus ja käytettävyys](guidance-compute-multi-vm.md)
- [Käynnissä olevan Windows-VMs N-taso-arkkitehtuuri](guidance-compute-n-tier-vm.md)
- [Käynnissä Linux VMs N-taso-arkkitehtuuri](guidance-compute-n-tier-vm-linux.md)
- [Käynnissä Windows VMs suuren useita alueilla](guidance-compute-multiple-datacenters.md)
- [Käynnissä Linux VMs suuren useita alueilla](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Yhteyden muodostaminen Azure paikalliseen verkkoon

Sarjassa osoittamaan olemassa olevan verkoston yhdistäminen Azure tapoja käynnistää. Valitse Näin saat kattavat käytettävyys ja suojausta koskevat vaatimukset.

- [Käyttöönoton hybrid-verkoston arkkitehtuuri ja Azure ja paikallisten VPN](guidance-hybrid-network-vpn.md)
- [Hybrid-verkoston arkkitehtuuri, Azure ExpressRoute ja toteuttaminen](guidance-hybrid-network-expressroute.md)
- [Käyttöönoton erittäin käytettävissä hybrid verkko-arkkitehtuuri](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Hybrid verkon suojaaminen

Tämä sarja kattaa osoitettu käytännöistä DMZ luominen Azure tulevat paikallisen palvelinkeskuksen ja Internetin suojattuja yhteyksiä.

- [Käyttöönoton DMZ Azure ja paikallisen palvelinkeskuksen välillä](guidance-iaas-ra-secure-vnet-hybrid.md)
- [DMZ Azure ja toteuttaminen](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Käyttäjätietojen palveluja

Tämä sarja käynnistää näytetään, miten voi käyttää Azure Active Directory käyttöoikeuksien Azure-tietokannassa. Valitse Näin saat kattavat monimutkaisia skenaariot laajentaminen Lisää infrastruktuurin Azure ja ADFS: N käytön delegointi.

- [Azure Active Directory toteuttaminen](./guidance-identity-aad.md)
- [Ulottuu Azure Active Directory-palveluihin (Lisää)](./guidance-identity-adds-extend-domain.md)
- [Azure Active Directory Directory Services (Lisää)-resurssin metsää luominen](./guidance-identity-adds-resource-forest.md)
- [Azure Active Directory-liittoutumispalvelut (ADFS) toteuttaminen](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Suunnittelee skaalattava verkkosovelluksen Azure PaaS

Tämä sarja kattaa suosituksia luomisesta skaalattava ja erittäin käytettävissä verkkosovelluksissa. 

- [Basic web-sovelluksen](guidance-web-apps-basic.md)
- [Web-sovelluksen skaalattavuus parantaminen](guidance-web-apps-scalability.md)
- [Web-sovelluksen ja suuri käytettävyys](guidance-web-apps-multi-region.md)
