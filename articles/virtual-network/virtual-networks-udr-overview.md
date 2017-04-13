<properties 
   pageTitle="Mitä käyttäjän määrittämät tiet, ja IP-osoitteen välityksen?"
   description="Opettele käyttämään käyttäjän määrittämät tiet (UDR), ja IP-osoitteen välityksen eteenpäin liikennettä verkon virtual laitteiden Azure-tietokannassa."
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

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Mitä käyttäjän määrittämät tiet, ja IP-osoitteen välityksen?
Kun lisäät näennäiskoneiden (VMs) Azure virtual verkon (VNet), huomaat, että VMs pystyvät keskenään verkon automaattisesti. Sinun ei tarvitse määrittää yhdyskäytävän, vaikka VMs eri aliverkosta. Sama koskee julkinen Internet VMs tietoliikenne ja jopa paikalliseen oman verkon, kun oman palvelinkeskukseen Azure hybrid yhteyden ei sisällä tietoja.

Tämä työnkulku viestinnän on mahdollista, koska Azure voit määrittää, kuinka IP-liikenne jatkuu järjestelmän tiet sarjaa. Järjestelmän tiet ohjataan viestintä seuraavissa tilanteissa:

- Valitse saman aliverkon sisällä.
- Aliverkosta toiseen VNet.
- Valitse VMs Internetiin.
- Valitse VNet, toinen VNet VPN-yhdyskäytävän kautta.
- Valitse VNet paikalliseen verkkoon VPN-yhdyskäytävän kautta.

Alla olevassa kuvassa VNet, kahden aliverkon ja muutamia VMs sekä järjestelmän tiet, joka mahdollistaa IP-liikenne paikalliseen flow yksinkertainen määritys.

![Järjestelmän tiet Azure-tietokannassa](./media/virtual-networks-udr-overview/Figure1.png)

Vaikka järjestelmän tiet käyttäminen helpottaa tietoliikenteen automaattisesti käyttöönoton, on tapauksia, joissa haluat hallita pakettien reititys virtual laitteen kautta. Voit niin luomalla käyttäjän määrittämät tiet, joka määrittää tietyn aliverkon Siirry virtual laitteen sijaan juoksutus ja ottamalla käyttöön käynnissä virtual laitteen AM edelleenlähetyksen IP-pakettien seuraavan kohteen.

Seuraavassa kuvassa on esimerkki käyttäjän määrittämät tiet ja IP-välityksen paketit lähetetään yhden aliverkon toisesta menee läpi virtual laitteen kolmannen aliverkon.

![Järjestelmän tiet Azure-tietokannassa](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Käyttäjän määrittämät tiet vain käytetään jätä aliverkon liikenne. Et voi luoda tiet, voit määrittää, miten liikenne tulee aliverkon Internetistä esiintymän. Lähetät-liikenne paikalliseen laitteen ei voi olla myös saman aliverkon, josta liikenne on peräisin. Luoda erilliset aliverkon oman laitteiden aina. 

## <a name="route-resource"></a>Reitin resurssi
Pakettien reititetään määritettyjen kunkin solmun fyysinen verkossa reittitaulukkoon perustuva TCP/IP-verkon kautta. Reitin taulukko on kokoelma yksittäisiä tiet käytettävä päätä Välitä paketteja kohde-IP-osoitteen perusteella. Reititys koostuu seuraavasti:

|Ominaisuus|Kuvaus|Rajoitukset|Huomioon otettavia seikkoja|
|---|---|---|---|
| Osoitteen etuliite | Kohde-CIDR, johon reitin koskee, kuten 10.1.0.0/16.|On on kelvollinen CIDR alue, jotka edustavat julkinen Internet-Azure virtual verkossa tai paikallisen palvelinkeskuksen osoitteet.|Varmista, että **osoitteen etuliite** ei sisällä osoite **Seuraava osoite**, muuten oman pakettien syöttää silmukan siirtymällä lähteestä seuraavan pisteen ilman kurottavat koskaan kohde. |
| Seuraavan kohteen tyyppi | Azure kohteen tyypin paketin pitäisi lähettää. | On oltava jokin seuraavista arvoista: <br/> **VPN**. Edustaa virtual lähiverkossa. Esimerkiksi jos kahden aliverkon, 10.1.0.0/16 ja 10.2.0.0/16 ovat samassa virtual verkossa, reitin kunkin aliverkon reitti-taulukossa on *VPN*seuraavan kohteen arvo. <br/> **VPN-yhdyskäytävä**. Edustaa Azure S2S VPN-yhdyskäytävän. <br/> **Internet**. Edustaa Azure-infrastruktuurin myöntämä oletusarvon Internet-yhdyskäytävä. <br/> **Virtuaalinen laitteen**. Edustaa virtual laitteen, lisäämäsi Azure virtual verkkoon. <br/> **Ei mitään**. Edustaa musta rei. Siirtää musta rei pakettien ei lähetetään edelleen ollenkaan.| Kannattaa käyttää **ei mitään** -tyypin lopettavat paketit juoksutus annetun kohteeseen. | 
| Seuraava osoite | Seuraava osoite sisältää pakettien kohdesähköpostiosoitetta IP-osoite. Seuraavan kohteen arvot sallitaan vain tiet, jossa seuraavan kohteen tyyppi on *Virtual laitteen*.| IP-osoite, joka on tavoitettavissa käyttäjän määrittämät reitin käytettyjen Virtual verkoston on oltava. | Jos IP-osoite AM, varmista, että otat käyttöön AM [IP-osoitteen välityksen](#IP-forwarding) Azure-tietokannassa. |

PowerShellin Azure joissakin "NextHopType" arvot on eri nimet:
- VPN on VnetLocal
- Virtuaalinen yhdyskäytävien on VirtualNetworkGateway
- Virtuaalinen laitteen on VirtualAppliance
- Internet on Internet-
- Ei mitään

### <a name="system-routes"></a>Järjestelmän tiet
Jokaisen aliverkon luotu virtual verkon liitetään automaattisesti reitti-taulukko, joka sisältää järjestelmän reitti-sääntöjä:

- **Paikallinen Vnet sääntö**: Tämä sääntö luodaan automaattisesti jokaisen aliverkon virtual verkossa. Se määrittää, että VMs VNet-välillä on suora linkki ja ei ole keskitason seuraavan kohteen.
- **Paikallisen säännön**: Tämä sääntö koskee kaikki liikenne paikalliseen osoitealueita tarkoitettu ja käyttää VPN-yhdyskäytävän kohteeksi seuraavan kohteen.
- **Internet-säännön**: tämän säännön kahvoja kaikki liikenne reitittämistä julkinen Internet (osoitteen etuliite 0.0.0.0/0) ja käyttää infrastruktuurin internet-yhdyskäytävä seuraavan pisteen kaikki liikenne reitittämistä Internetiin.

### <a name="user-defined-routes"></a>Käyttäjän määrittämät tiet
Useimmat ympäristössä tarvitset vain jo määrittämiä Azure järjestelmän tiet. Voit joutua kuitenkin reititystaulukon luominen ja Lisää vähintään yksi tiet kuten tietyissä tapauksissa:

- Voit pakottaa tunneling Internet paikallisen verkon kautta.
- Käytä virtual laitteiden Azure-ympäristössä.

Yllä skenaarioissa on reititystaulukon luominen ja käyttäjän määrittämät tiet lisääminen. Voi olla useita Reitin taulukoita ja reitin saman taulukon voi liittää vähintään yksi aliverkosta. Ja kunkin aliverkon vain voi liittää yhteen reitin taulukkoon. Kaikki VMs ja pilvipalveluihin aliverkon käytössä reititystaulukon liittyvät kyseisen aliverkon.

Aliverkosta luottavat järjestelmän tiet, kunnes reititystaulukon liitetään aliverkon. Kun yhteys on luotu, reititys on valmis perusteella-pisimmän etuliitteen vastine (LPM) käyttäjän määrittämät tiet ja järjestelmän tiet välillä. Jos on useampi kuin yksi reitin saman LPM vastine reitin valitaan perusteella alkuperän seuraavassa järjestyksessä:

1. Käyttäjälle määritettyä reitin
1. Erityisen reitin (kun ExpressRoute on käytössä)
1. Järjestelmän reitin

Lisätietoja siitä, miten voit luoda käyttäjän määrittämiä, katso, [miten voit luoda tiet ja ota käyttöön IP edelleen Azure-tietokannassa](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] Käyttäjän määrittämät tiet käytetään vain Azure VMs ja cloud Services-palveluun. Esimerkiksi jos haluat lisätä palomuurin virtual laitteen paikalliseen verkkoon ja Azure välillä, sinun on luoda käyttäjän määrittämiä reitti Azure reitin taulukoissa, joka välittää kaikki liikenne virtual laitteeseen paikallisen osoite-kohtaan. Käyttäjän määrittämät reititys (UDR) voit lisätä myös GatewaySubnet Siirrä kaikki liikenne paikalliseen Azure virtual laitteen kautta. Tämä on äskettäin lisätty.

### <a name="bgp-routes"></a>Erityisen tiet
Jos sinulla on ExpressRoute yhteys paikalliseen verkkoon ja Azure välillä, voit ottaa erityisen leviäminen tiet, Azure paikallisen verkosta. Nämä erityisen tiet käytetään samalla tavalla kuin järjestelmän tiet ja käyttäjän määrittämät reitittää-kunkin Azure aliverkon. Katso lisätietoja [ExpressRoute esittely](../expressroute/expressroute-introduction.md).

>[AZURE.IMPORTANT] Voit määrittää Azure ympäristön käyttämään voimassa tunneling paikallisen verkon kautta luomalla käyttäjän määritetyn reitin aliverkon 0.0.0.0/0, joka käyttää VPN-yhdyskäytävän seuraavan kohteen. Kuitenkin Tämä toimii vain, jos käytössäsi on VPN-yhdyskäytävän ei ExpressRoute. ExpressRoute pakotettu tunneling on määritetty erityisen kautta.

## <a name="ip-forwarding"></a>IP-välityksen
Kuvaavat yläpuolelle, kun jokin tärkeintä syytä luo käyttäjälle määritettyä reititys on liikenteen virtual laitteeseen. Virtuaalinen laitteen ei ole enää mitään muuta kuin AM, joka suoritetaan, sovellus käyttää käsittelyyn verkkoliikennettä jollakin tavalla, kuten palomuurin tai NAT-laitteen.

Tämä virtual laitteen AM on oltava vastaanottaa saapuvan liikenteen, joka ei ole itse osoitettu. Anna AM vastaanottamaan tietoliikennettä osoitettu muihin kohteisiin, on otettava käyttöön IP-osoitteen välityksen AM. Tämä on Azure asetusta, ei Vieras-käyttöjärjestelmässä.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [tiet Resurssienhallinta käyttöönoton mallin](virtual-network-create-udr-arm-template.md) luomisesta ja aliverkosta toisiinsa. 
- Lisätietoja [tiet perinteinen käyttöönotto-mallin](virtual-network-create-udr-classic-ps.md) luominen ja aliverkosta toisiinsa.
