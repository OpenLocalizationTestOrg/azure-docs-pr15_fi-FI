<properties
    pageTitle="Suunnitteleminen Azure RemoteApp-sivustokokoelman virtual verkon | Microsoft Azure"
    description="Opettele luomaan virtual verkon Azure RemoteApp-sivustokokoelman."
    services="remoteapp"
    documentationCenter="" 
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Voit suunnitella virtual verkon Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Tässä asiakirjassa kerrotaan, miten Azure virtual verkon (VNET) ja aliverkon määritetään Azure RemoteApp. Jos olet perehtynyt Azure virtual verkkojen, tämä on ominaisuus, joka auttaa virtualisoi pilveen verkkoinfrastruktuuria ja luo hybrid ratkaisuja Azure ja paikallisen resurssit. Voit lukea lisää se [tähän](../virtual-network/virtual-networks-overview.md).

Jos haluat määrittää suojauskäytäntöjä tietoliikenteen (saapuvat ja lähtevät) Kun otat Azure RemoteApp virtual verkossa, suosittelemme luominen eri aliverkon Azure RemoteApp muusta käyttöönoton Azure virtual verkossa. Lisätietoja siitä, miten voit määrittää suojauskäytäntöjä Azure virtual verkko-osoitteiden Lue [verkon suojauksen ryhmän (NSG) ominaisuudet?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Azure RemoteApp sivustokokoelmat Azure virtual verkkojen kanssa tyypit

Seuraavissa kuvissa Näytä sivustokokoelman eri vaihtoehdoista, kun haluat käyttää virtual verkkoon.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp cloud sivustokokoelman VNET kanssa

 ![Azure RemoteApp - pilvi kokoelma, jossa VNET](./media/remoteapp-planvpn/ra-cloudvpn.png)

Tämä vastaa Azure RemoteApp-sivustokokoelman, jossa kaikki resurssit, joiden RemoteApp-istunnon isännät käyttämään otetaan käyttöön Azure-tietokannassa. Ne voi olla samaa VNET kuin RemoteApp VNET tai eri VNET Azure-tietokannassa.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure RemoteApp hybrid sivustokokoelman VNET kanssa

![Azure RemoteApp - Hybrid kokoelma, jossa VNET](./media/remoteapp-planvpn/ra-hybridvpn.png)

Tämä vastaa joistakin resurssit, joiden RemoteApp istunnon isännät käyttämään sijainti käyttöön paikallisen Azure RemoteApp-sivustokokoelman. RemoteApp-VNET on linkitetty käyttöä Azure hybrid toiminnot, kuten sivuston sivuston VPN- tai Express reitin paikalliseen verkkoon.


## <a name="how-the-system-works"></a>Järjestelmän toiminta

Kohdassa kattaa Azure RemoteApp ottaa käyttöön, jonka valitsit valmistelun aikana VPN-aliverkon Azuren näennäiskoneiden (ja ladatut kuva). Jos olet valinnut hybrid kokoelman, emme yrittää ratkaista FQDN toimialueen ohjauskoneen kirjoitit valmistelu työnkulun annettu virtual verkon DNS-palvelimeen.  
Jos olet muodostamassa virtual verkkoon, varmista, että voit näyttää verkon suojauksen ryhmien-Azure RemoteApp aliverkon tarvittavat portit. 

Suosittelemme, että käytössäsi on [riittävän suuri aliverkon Azure RemoteApp](remoteapp-vnetsizing.md). Suurin Azure VPN tukemat on /8 (joko CIDR aliverkon määritelmät). Käytettävissä myös aliverkon ulkopuolella on oltava mahtuu Azure RemoteApp VMs kaikki asteikon ylöspäin aikana, kun useita käyttäjiä käyttöoikeuspalvelinta sovellukset. 

Sinun täytyy ottaa käyttöön VPN-aliverkon seuraavia asioita: 

2.  Lähtevän tietoliikenteen aliverkon olisi sallittava porttialueen 10101 10175 kommunikoimiseen jonkin sisäinen Azure RemoteApp-palvelujen kanssa.
3.  Lähtevän tietoliikenteen voitava oman aliverkosta Azuren tallennustilaan porttiin 443 yhdistäminen
4.  Jos sinulla on ylläpidettävä Azure Active Directory, varmista, että kaikki AM Azure RemoteApp VPN-aliverkon sisällä on pystyttävä muodostamaan yhteys kyseisen toimialueen ohjauskoneen. Virtuaalinen verkon DNS pitäisi voit ratkaista tämän toimialueen ohjauskoneen täydellinen toimialuenimi.


## <a name="virtual-network-with-forced-tunneling"></a>Virtuaalinen verkostoa pakotettu tunneling

[Pakotettu tunneling](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) tukee nyt kaikki uudet Azure RemoteApp sivustokokoelmat. Microsoft ei tue aiemmin sivustokokoelmassa tukemaan pakotettu tunneling siirron.  Sinun on Poista kaikki käyttämällä VNET, johon linkität Azure RemoteApp aiemmin sivustokokoelmat ja luoda uuden Hae pakotettu tunneling otettu käyttöön oman sivustokokoelmat. 
