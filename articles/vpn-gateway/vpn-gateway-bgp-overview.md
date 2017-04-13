<properties
   pageTitle="Yleistä erityisen kanssa Azure VPN yhdyskäytävien | Microsoft Azure"
   description="Tässä artikkelissa on yleiskatsaus erityisen Azure VPN yhdyskäytävien kanssa."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Yleistä erityisen Azure VPN yhdyskäytävien kanssa

Tässä artikkelissa on yleiskatsaus Azure VPN yhdyskäytävien erityisen (reunan yhdyskäytävän Protocol)-tuen.

## <a name="about-bgp"></a>Lisätietoja erityisen

Erityisen on vakio reititys-protokolla käytettyjä Internet Exchange reititys ja syy tietoja kahden tai useamman välillä. Azure Virtual verkostojen kontekstissa käytettäessä erityisen avulla Azure VPN-yhdyskäytävien ja paikallisen VPN laitteilla, kutsutaan erityisen kollegat tai muiden tekijöiden, Exchange "reitittää", joka ilmoittaa sekä yhdyskäytävien saatavuus ja näiden etuliitteiden käsitellään yhdyskäytävien tai reitittimen syy toimenpiteitä. Erityisen voit myös ottaa salataanko siirrettävät reititys kesken useiden verkostojen mukaan välittäminen tiet erityisen yhdyskäytävän oppii yhden erityisen peer-kaikki erityisen kollegat.
 
### <a name="why-use-bgp"></a>Erityisen käyttötarkoitus

Erityisen on valinnainen ominaisuus, voit käyttää Azure reitti-pohjainen VPN yhdyskäytävät. Voit myös Varmista paikallisen VPN-laitteet tukevat erityisen, ennen kuin otat käyttöön. Voit edelleen käyttää Azure VPN yhdyskäytävien ja paikallisen VPN-laitteiden ilman erityisen. Se on staattinen tiet (ilman erityisen) *ja* käyttämällä dynaaminen reititys kanssa erityisen verkkojen ja Azure välillä käyttämällä vastikkeen.

On useita hyötyjä ja erityisen uusia ominaisuuksia:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Automaattiset ja joustavia etuliite päivitykset tuki

Erityisen, jossa on vain haluat määritellä pienin etuliite tietyn erityisen vertaisjärjestelmä IP S2S VPN-tunnelin kautta. Voi olla mahdollisimman pieni host etuliite (/ 32) paikallisen VPN-laitteesi erityisen peer IP-osoite. Voit hallita joka paikalliseen verkkoon etuliitteiden haluat ilmoittaa Azure sallimaan Azure Virtual verkon käyttämään.
    
Voit myös mainostaa suurempi etuliitteiden, jotka voivat sisältää joitakin oman VNet-osoitteiden etuliitteiden, kuten suuri yksityinen IP address välilyönti (esimerkiksi 10.0.0.0/8). Huomaa, vaikka etuliitteitä ei saa olla sama jokin VNet etuliitteitä. Sama kuin VNet etuliitteiden reitillä hylätään.

>[AZURE.IMPORTANT] Tällä hetkellä mainonta Azure VPN yhdyskäytävien oletusarvon reititys (0.0.0.0/0) estetään. Lisätietoja päivitys toimitetaan, kun tämä ominaisuus on otettu käyttöön.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Tukee useita tunneleita VNet ja paikallisen-sivuston ja automaattinen automaattisesti erityisen perusteella

Voit muodostaa useita yhteyksiä Azure VNet ja paikallisen VPN-laitteiden samassa sijainnissa välillä. Tämä ominaisuus on useita tunneleita (polut) aktiivinen aktiivinen-kokoonpanossa kahden verkon välillä. Jos jokin akselitunnelien katkeaa, vastaava tiet peruutetaan kautta erityisen ja liikenne siirtyvät automaattisesti jäljellä tunneleita.
    
Seuraavassa kaaviossa on esitetty Esimerkki erittäin käytettävissä asennus:
    
![Useita aktiivinen polkuja](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Tue salataanko siirrettävät reititys paikallisen lopettaminen ja useita Azure VNets välillä

Erityisen mahdollistaa useita yhdyskäytäviä ja välittäminen etuliitteiden eri verkostoista, onko ne suoraan tai epäsuorasti yhteydessä. Voit ottaa salataanko siirrettävät reititys Azure VPN yhdyskäytävien paikallisen sivustojen välillä tai useiden Azure Virtual verkkojen kanssa.
    
Seuraavassa kaaviossa on esitetty Esimerkki usean kohteen topologian, useita liikeradan, jotka voivat kulkea liikenne kahden paikallisen verkon kautta Azure VPN yhdyskäytävien kuluessa Microsoft Networks välillä:

![Usean kohteen salataanko siirrettävät](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>Erityisen usein kysyttyjä kysymyksiä


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Seuraavat vaiheet

Katso [aloittaminen erityisen Azure VPN yhdyskäytävät-](./vpn-gateway-bgp-resource-manager-ps.md) ohjeita paikallisen- ja VNet VNet yhteydet erityisen määrittämiseksi.

