
<properties
   pageTitle="Azure virtual verkon peering | Microsoft Azure"
   description="Lisätietoja peering Azure VNet."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>VNet peering

VNet peering on toimintoa, joka yhdistää kaksi virtual verkkoa (VNets) samalla alueella Azure kuvaa runkoverkkoa verkon kautta. Kun peered, kaksi virtual verkot tulevat näkyviin yksi yhteyden kaikkiin tarkoituksiin. Niitä hallitaan silti erillisessä resursseina, mutta näitä virtual verkoissa näennäiskoneiden viestiä keskenään suoraan yksityisten IP-osoitteiden avulla.

Näennäiskoneiden peered virtual verkkojen välillä liikenne reititetään Azure infrastruktuurin samalla tavalla kuin liikenne reititetään VMs virtual samassa verkostossa välillä. Muun muassa seuraavat VNet peering eduista:

- Pieni viive, suuren kaistanleveyden välinen yhteys eri virtual verkkoihin resurssit.
- Mahdollisuus käyttää resurssit, kuten verkon laitteiden ja VPN yhdyskäytävien peered VNet salataanko siirrettävät asioista.
- Mahdollisuus yhdistää virtual verkkoon, joka käyttää virtual verkkoon, joka käyttää perinteinen käyttöönottomalli ja ota käyttöön koko virtual verkostojen resurssien väliset yhteydet Azure Resurssienhallinta-malli.

Vaatimukset ja VNet peering keskeisiä ominaisuuksia:

- Kaksi virtual verkkoja, jotka ovat peered on oltava samassa Azure alueella.
- Virtuaalinen verkkoja, jotka ovat peered pitäisi olla ikkunat IP-osoite välilyöntejä.
- VNet peering on kaksi virtual verkkojen välillä ja ei ole johdettu transitiiviverbien yhteyttä. Esimerkiksi jos virtual verkon A on peered virtual verkon B ja virtual verkon B on peered virtual verkon C-näppäinyhdistelmää, jos se ei kääntää, virtual verkko parhaillaan peered virtual verkon C.
- Peering voi muodostaa kahden eri tilaukset virtual verkot välillä pitkään sekä tilaukset sellaisten käyttäjä vahvistaa peering ja tilaukset liittyvät saman Active Directory-vuokraajan. 
- Resurssien hallinnan malli virtual verkon ja perinteinen käyttöönoton mallin välisten peering edellyttää, että VNets pitäisi olla saman tilauksen.
- Virtuaalinen verkkoon, joka käyttää resurssien hallinnan käyttöönottomalli voit peered toiseen virtual verkossa, jossa käytetään tätä mallia tai virtual verkoston kanssa, joka käyttää perinteinen käyttöönottomalli. Perinteinen käyttöönoton mallin käyttävät virtual verkot et voi kuitenkin peered toisiinsa.
- Vaikka välisen peered virtual verkoissa näennäiskoneiden on kaistanleveyden ei rajoituksia, kaistanleveyden pää AM koon perusteella koskee edelleen.


![Tavallinen VNet peering](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Yhteys
Kun kaksi virtual verkot ovat peered, virtual machine (web/Työntekijä rooli) virtual verkon suoraan voivat muodostaa yhteyden muiden näennäiskoneiden peered virtual verkossa. Nämä kaksi verkot on koko IP-tason yhteys.

Verkon latenssin kierron kaksi näennäiskoneiden peered virtual verkkojen välillä, on sama kuin kierron virtual lähiverkossa kuluessa. Verkon suorituskykyä perustuu kaistanleveyden, jota sallittu kuvareunuksen koon virtuaalikoneen. Sovelluksessa ei ole muita kaistanleveyden rajoituksen.

Näennäiskoneiden peered virtual verkkojen välillä-liikenne reititetään suoraan Azure taustatietokantaan infrastruktuuri ja ei yhdyskäytävän kautta.

Näennäiskoneiden virtual verkossa, voit käyttää sisäinen kuormituksen (ILB) päätepisteet peered virtual verkossa. Verkon käyttöoikeusryhmät (NSGs) voi suojata joko virtual verkossa muiden virtual verkkoja tai aliverkosta käytön estäminen tarvittaessa.

Kun käyttäjät määrittäminen peering, ne Avaa tai sulje NSG säännöt virtual verkkojen välillä. Jos käyttäjä valitsee Avaa koko peered virtual verkkojen (joka on oletusasetus) väliset yhteydet, he voivat käyttää NSGs tietyn aliverkosta tai näennäiskoneiden estosta tai kieltää tietyn.

Jos Azure sisäiseen DNS-nimenselvitys näennäiskoneiden ei toimi peered virtual verkkojen. Näennäiskoneiden on sisäinen DNS-nimet, jotka on ratkaistava vain virtual lähiverkossa sisällä. Käyttäjät voivat kuitenkin määrittää näennäiskoneiden, joissa on käytössä peered virtual verkoissa kuin virtual verkon DNS-palvelimet.

## <a name="service-chaining"></a>Palvelun ketjutus
Käyttäjät voivat määrittää käyttäjän määrittämä reititystaulukot, joka osoittaa näennäiskoneiden peered virtual verkoissa nimellä "seuraavan pisteen" IP-osoite, tämän artikkelin kaavio esitetyllä tavalla. Näin käyttäjät saavuttamiseksi palvelun ketjutus, jonka kautta ne suoraan liikenne virtual verkosta virtual laitteeseen, jossa on käytössä peered virtual verkon käyttäjän määrittämä reitin taulukoista.

Käyttäjät voivat muodostaa myös tehokkaasti keskittimeen ja puhui tyyppi ympäristössä, jossa valitsemalla ylläpitää infrastruktuurin osia, kuten verkon virtual laitteen. Puhui kaikki virtual verkot voit sitten vertaiskone se sekä alijoukkoa-liikenne paikalliseen laitteita, joissa on keskittimeen virtual verkon kanssa. Lyhyesti sanottuna VNet peering avulla 'käyttäjän määrittämät reitin taulukko' seuraavan kohteen IP-osoite on virtual koneen peered virtual verkon IP-osoite.

## <a name="gateways-and-on-premises-connectivity"></a>Yhdyskäytävien ja paikallisen yhteys
Kunkin virtual verkon, riippumatta siitä, onko se peered toiseen virtual verkoston kanssa, voit silti on oma yhdyskäytävän ja käyttää sitä Muodosta yhteys paikalliseen. Käyttäjät voivat myös määrittää [VNet VNet yhteydet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) käyttämällä yhdyskäytäviä, vaikka virtual verkot ovat peered.

Kun VPN siksi molemmat asetukset on määritetty, virtual verkkojen välillä liikenne kulkee peering määritys (eli Azure kuvaa runkoverkkoa kautta).

Kun virtual verkot ovat peered, käyttäjät voivat myös määrittää yhdyskäytävän peered virtual verkon salataanko siirrettävät-kohdan paikalliseen. Tässä tapauksessa virtual verkko, jossa on käytössä remote yhdyskäytävä ei voi olla oma yhdyskäytävä. Yksi virtual verkon voi olla vain yksi yhdyskäytävä. Ne voivat olla paikallisen yhdyskäytävän tai (peered virtual verkon) remote yhdyskäytäviä seuraavassa kuvassa esitetyllä tavalla.

Yhdyskäytävän salataanko siirrettävät ei tue virtual verkkojen Resurssienhallinta-malli ja ne perinteinen käyttöönoton mallin peering suhteen. Virtuaalinen verkkojen peering yhteyteen on käyttäminen yhdyskäytävän salataanko siirrettävät Resurssienhallinta käyttöönoton mallista.

Kun virtual verkkoja, jotka jakamassa yksittäisen Azure ExpressRoute yhteyden ovat peered, niiden välinen tietoliikenne käy läpi peering suhteen (eli Azure kuvaa runkoverkkoa verkon kautta). Käyttäjät voivat silti käyttää paikallisen yhdyskäytävät kunkin virtual verkon Muodosta yhteys paikalliseen piiri. Voit myös ne käyttää jaettua yhdyskäytävän ja määritä salataanko siirrettävät paikallisen yhteyksiä varten.

![VNet peering salataanko siirrettävät](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Valmistelu
VNet peering on sellaisten toiminto. On erillinen funktioon VirtualNetworks nimitilan. Käyttäjä voi antaa oikeudet sallivat peering. Käyttäjä, jolla on luku-ja kirjoitusoikeudet virtual verkon perii näiden oikeuksien automaattisesti.

Käyttäjä, joka on joko järjestelmänvalvoja tai peering mahdollisuuden sellaisten käyttäjä aloittaa peering toiminnon, valitse toinen VNet. Jos vastaavaa pyytää, valitse toinen puoli peering ja muut vaatimukset täyttyvät, peering muodostetaan.

Lisätietoja saat lisätietoja muodostaminen VNet peering kaksi virtual verkkojen välillä "Seuraava vaiheet"-osion artikkelit.

## <a name="limits"></a>Rajoitukset
On peerings, joita voi yhden virtual verkoston enimmäismäärään. Viittaavat [Azure verkko rajoitukset](../azure-subscription-service-limits.md#networking-limits) lisätietoja.

## <a name="pricing"></a>Hinnat
VNet peering ovat maksuttomia tarkistuksen aikana. Kun se on julkaistu, tunkeutumisen ja ulospääsy liikenteen, joka hyödyntää peering ole korko.vuosi maksu. Saat lisätietoja viitata [hinnat sivun](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="next-steps"></a>Seuraavat vaiheet
- [Määritä peering virtual verkkojen välillä](virtual-networks-create-vnetpeering-arm-portal.md).
- Lisätietoja [NSGs](virtual-networks-nsg.md).
- Lisätietoja [käyttäjän määrittämä tiet ja IP-välityksen](virtual-networks-udr-overview.md).
