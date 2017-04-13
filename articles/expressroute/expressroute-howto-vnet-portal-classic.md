<properties
   pageTitle="VPN- ja yhdyskäytävän määrittäminen ExpressRoute perinteinen portaalissa | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi perinteinen käyttöönottomalli ja perinteinen portaalin ExpressRoute virtual verkon määrittäminen."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Luo virtuaalisia verkon ExpressRoute perinteinen-portaalissa

Tässä artikkelissa kuvattuja edetään ohjatusti määrittäminen perinteinen käyttöönottomalli ja perinteinen portaalin ExpressRoute virtual verkko- ja VPN-yhdyskäytävän käyttöä varten.

Jos etsit ohjeita resurssien hallinnan käyttöönottomalli, voit käyttää on seuraavissa artikkeleissa: [Luo virtual verkon PowerShellin avulla](../virtual-network/virtual-networks-create-vnet-arm-ps.md) ja [Lisää VPN-yhdyskäytävän Resurssienhallinta-VNet ExpressRoute varten](expressroute-howto-add-gateway-resource-manager.md).

**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Perinteinen VNet ja yhdyskäytävän luominen

Perinteinen VNet ja virtual yhdyskäytävien luominen seuraavien ohjeiden mukaisesti. Jos sinulla on jo perinteinen VNet, katso tämän artikkelin [Configure aiemmin perinteinen VNet](#config) -kohtaan.

1. Kirjaudu sisään [Azure perinteinen portal](http://manage.windowsazure.com).

2. Valitse näytön vasemmassa alakulmassa **Uusi**. Valitse siirtymisruudussa **Verkkopalveluita**ja valitse sitten **VPN**. Valitse Aloita ohjattu määritystoiminto **Mukautettu luominen** .

3. Anna **Virtual verkon tiedot** -sivulla seuraavasti:

    - **Nimi** – virtual verkon nimi. Käytät tätä VPN-nimeä, kun otat käyttöön VMs ja PaaS esiintymät, joten et ehkä halua tehdä liian monimutkaiselta nimi.
    - **Sijainti** : sijainti suoraan liittyvät resurssit (VMs) sijaitsevan haluamaasi fyysinen sijainti (alue). Esimerkiksi jos haluat VMs, sijoitettava fyysisesti Yhdysvaltojen Itä virtual verkoston käyttöön, valitse kyseiseen sijaintiin. Et voi muuttaa alueen liittyvät virtual verkon sen luomisen jälkeen.

4. Valitse **DNS-palvelimet ja VPN-yhteys** -sivulla seuraavat tiedot ja valitse sitten oikeassa alakulmassa seuraava-nuoli. 

    - **DNS-palvelimet** - DNS-palvelimen nimi ja IP-osoite tai valitse aiemmin rekisteröidyn DNS-palvelin pikavalikosta. Tämä asetus luo DNS-palvelimeen. Sen avulla voit määrittää DNS-palvelimet, jota haluat käyttää virtual verkoston nimenselvitys.
    - **Sivuston sivuston yhteys** - **Määritä sivusto sivusto VPN-yhteyttä**, valitse valintaruudun valinta.
    - **ExpressRoute** – Valitse **Käytä ExpressRoute**-valintaruutu. Tämä asetus näkyy vain, jos olet valinnut **Määritä sivusto sivusto VPN-yhteyttä**.
    - **Lähiverkossa** - on lähiverkossa sivuston ExpressRoute tarvitaan. Kuitenkin kyseessä on ExpressRoute yhteys määritetty lähiverkossa-sivuston osoitteen etuliitteiden ohitetaan. Sen sijaan ilmoitetaan Microsoft kautta ExpressRoute piiri osoite etuliitteitä käytetään reititys tarkoituksiin.<BR>Jos sinulla on jo luotu ExpressRoute yhteys paikalliseen verkkoon, voit valita avattavasta valikosta. Jos et, valitse **Määritä uusi paikalliseen verkkoon**.

5. **Sivuston sivuston yhteys** -sivu tulee näkyviin, jos olet valinnut Määritä uusi lähiverkossa edellisessä vaiheessa. Määrittäminen paikallisessa verkossa, seuraavat tiedot ja valitse sitten Seuraava-nuoli. 

    - **Nimi** - nimi, jolle haluat soittaa paikallisesta (paikallisen) verkon sivuston.
    - **Osoitetilan** - mukaan lukien käynnistäminen IP-osoite ja CIDR (osoite määrä). Voit määrittää minkä tahansa osoitealueita, kunhan se peittää osoite solualueen virtual verkkoa varten. Tavallisesti Tämä määrittää paikallisen lopettaminen osoitealueet, mutta kyseessä ExpressRoute-asetuksia ei käytetä. Kuitenkin tämän asetuksen vaaditaan, jotta luominen lähiverkossa, kun käytät perinteistä portaalin.
    - **Lisää osoitetilaa** - asetusta ei ole merkitystä ExpressRoute.


6. Valitse **Välilyöntejä Virtual verkko-osoite** -sivulla seuraavat tiedot ja valitse sitten oikeassa alakulmassa verkon määrittäminen valintamerkki. 

    - **Osoitetilan** - mukaan lukien käynnistäminen IP-osoite ja määrä. Varmista, että määrität osoite välilyöntejä ole päällekkäin jonkin osoite välilyöntejä, joihin sinulla on paikallisessa verkossa.
    - **Lisää aliverkon** - mukaan lukien käynnistäminen IP-osoite ja osoitteiden määrä. Lisää aliverkosta eivät ole pakollisia.
    - **Lisää yhdyskäytävän aliverkon** - yhdyskäytävä-aliverkon Lisää napsauttamalla. Yhdyskäytävän aliverkon käytetään vain VPN-yhdyskäytävän ja määritysten tarvitaan.<BR>Yhdyskäytävän aliverkon CIDR (osoite määrä), ExpressRoute on oltava /28 tai suurempi (/ 27/26 jne.). Näin tarpeeksi IP-osoitteet, aliverkon toimimaan määrittämisen. Perinteinen-portaaliin, jos olet valinnut valintaruutu, jos haluat käyttää ExpressRoute-portaalin määrittää yhdyskäytävän aliverkon /28 kanssa.  Et voi muuttaa CIDR osoitteiden määrä perinteinen-portaalissa. Yhdyskäytävän aliverkon näkyy **yhdyskäytävän** perinteinen-portaalissa, vaikka yhdyskäytävän aliverkon, joka on luotu reaali nimi on todella **GatewaySubnet**. Voit tarkastella tätä nimeä PowerShell-toiminnolla tai Azure-portaalissa.

7. Valitse sivun alareunassa valintamerkki ja virtual verkon alkaa luomiseen. Kun se on valmis, näet **luotu** kohdasta **tila** **verkot** -sivulla perinteinen portaalissa.

## <a name="gw"></a>Yhdyskäytävän luominen

1. **Verkkojen** sivulla Valitse juuri luomasi virtual verkko ja valitse sitten **raporttinäkymät-ikkunan** sivun yläreunassa.

2. **Raporttinäkymät-ikkunan** sivun alareunaan, valitse **Luo yhdyskäytävä** ja valitse **Dynaaminen reititys**. Valitse Vahvista, että haluat luoda yhdyskäytävän **Kyllä** .

3. Kun yhdyskäytävän aloittaa luominen, näkyviin tulee viesti-salliminen tiedät, että yhdyskäytävä on käynnistetty. Voi kestää jopa 45 minuuttia yhdyskäytävän luominen.

4. Linkittäminen verkossa piirin. Noudata artikkelissa [VNets ExpressRoute piirit linkittämisestä](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Määrittää olemassa olevan perinteinen VNet ExpressRoute

Jos sinulla on jo perinteinen VNet, voit määrittää muodostaa yhteyden ExpressRoute perinteinen-portaalissa. Asetukset ovat samat kuin edellä kuvattujen, niin kuvattuihin osat tutustutaan tarvittavat asetukset. Jos haluat luoda samanaikaisesti olemassa olevien ExpressRoute /-,-sivusto-yhteyden, on [tämän artikkelin](expressroute-howto-coexist-classic.md) ohjeiden mukaisesti. He ovat erilaisia kuin tässä artikkelissa kuvattuja.
 
1. Sinun täytyy luoda paikallisen verkon, ennen kuin muiden VNet-asetusten päivittäminen. Voit luoda uuden paikalliseen verkkoon, jossa on tehtävä, määritettäessä ExpressRoute palvelun perinteinen-portaalissa, valitsemalla **Uusi** **>** **Verkkopalvelujen** **>** **VPN** **>** **Lisää paikalliseen verkkoon kirjauduttaessa**. Voit luoda paikallisen verkon noudattamalla ohjatun toiminnon.

2. Käytä **Määritä** sivun päivittää oman VNet asetusten loput ja liittää VNet paikalliseen verkkoon.

3. Kun asetusten määrittäminen Siirry tämän artikkelin yhdyskäytävän luominen [Luo yhdyskäytävä](#gw) -osassa.


## <a name="next-steps"></a>Seuraavat vaiheet

- Jos haluat lisätä näennäiskoneiden virtual verkkoon, katso [näennäiskoneiden oppimispolut](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Jos haluat lisätietoja ExpressRoute, katso [ExpressRoute yleiskatsaus](expressroute-introduction.md).


 
