<properties 
   pageTitle="Verkon liittymät | Microsoft Azure"
   description="Lisätietoja Azure verkkoliittymät Azure Resurssienhallinta."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Verkon liityntäkohdat

Verkko-käyttöliittymä (NIC) on yhteenliittämistä virtuaalikoneen (AM) ja ohjelmiston verkon välillä. Tässä artikkelissa kerrotaan, mitä verkon käyttöliittymä on ja miten sitä käytetään Azure resurssien hallinnan käyttöönottomalli.

Microsoft suosittelee käyttöönotto Resurssienhallinta käyttöönoton mallin uudet resurssit, mutta voit myös ottaa käyttöön VMs verkkoyhteyden [perinteinen](virtual-network-ip-addresses-overview-classic.md) käyttöönoton mallin kanssa. Jos ole aiemmin käyttänyt perinteistä mallin, on tärkeää eroja AM verkko-resurssien hallinnan käyttöönottomalli. Lue lisää välisiin eroihin [virtuaalikoneen verkko - perinteinen](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) -artikkelissa.

Valitse Azure-verkkoliittymän:

1. Resurssi voidaan luoda, poistaa, ja on oma määritettävät asetukset.
2. On oltava yhdistettynä yhden aliverkon yhden Azure Virtual verkossa (VNet) luomisen yhteydessä. Jos et ole aiemmin käyttänyt VNets, katso lisätietoja niitä lukemalla artikkelin [Virtual yleistä](virtual-networks-overview.md) . Verkkokortti on oltava yhdistettynä VNet, joka on saman Azure [sijainti](https://azure.microsoft.com/regions) ja [tilauksen](../azure-glossary-cloud-terminology.md#subscription) NIC-sivustosta. Kun NIC: ssä on luotu, voit muuttaa aliverkon, se on yhdistetty, mutta ei voi muuttaa VNet, se on yhdistetty.
3. On nimi myöntänyt siihen, jota ei voi muuttaa Verkkokortti luomisen jälkeen. Nimen on oltava yksilöllinen Azure [resurssiryhmä](../azure-resource-manager/resource-group-overview.md#resource-groups), mutta ei tarvitse olla yksilöivä tilaus, se on luotu Azure sijainnista ja se on yhdistetty VNet. Useita NIC luodaan yleensä Azure-tilauksen piiriin kuuluvien. On suositeltavaa suunnittelemaan nimeämiskäytäntöä, joka tekee useita NIC helpompaa kuin oletusnimet hallinta. On artikkelissa [Azure resurssien nimeämiskäytännöt suositellut](../guidance/guidance-naming-conventions.md) ehdotuksia.
4. Voidaan liittää AM, mutta voidaan liittää vain yhden AM, joka on samassa sijainnissa kuin NIC-sivustosta.
5. On MAC-osoite, joka on samanlainen varten NIC, kunhan yhä liitettynä AM. MAC-osoite on samanlainen, onko AM käynnistetään (-käyttöjärjestelmässä) tai pysäyttää (Poista kohdistettu) ja käyttäminen Azure-portaalissa, PowerShellin Azure tai Azure käyttöliittymä. Jos se on irrotettu AM ja liittää eri AM, Verkkokortti vastaanottaa eri MAC-osoite. Jos Verkkokortti on poistettu, MAC-osoite on liitetty muita NIC.
6. On yksi ensisijainen **Yksityinen** *IPv4* staattinen tai dynaaminen IP-osoitteen liittänyt siihen.
8. Saattaa yhden julkiseen IP-osoite resurssin liittyä siihen.
9. Tukee nopeutettu verkko yhden pääkansion i/o virtualization (SR IOV) tietyt AM koot tiettyä Microsoft Windows Server-käyttöjärjestelmä-versioiden kanssa. Saat lisätietoja tämän ESIKATSELU-toimintoa artikkeliin [Accelerated verkko virtual tietokoneelle](virtual-network-accelerated-networking-powershell.md) .
10. Voit vastaanottaa liikenne ei ole tarkoitettu yksityisten IP-osoitteiden määritetty Jos IP-välityksen on otettu käyttöön NIC-sivustosta. Jos AM ollessa käynnissä palomuuriohjelmiston esimerkiksi reitittää paketit muuksi kuin Oma IP-osoitteet. AM edelleen voi käyttää ohjelmistoa voi reititys tai välitettäessä liikenne, mutta voit tehdä IP-välityksen on oltava käytössä NIC-sivustosta.
11. Usein luodaan samaan resurssiryhmä se on liitetty AM tai saman VNet, johon se on kytketty, mutta se ei tarvitse olla.

## <a name="vms-with-multiple-network-interfaces"></a>VMs useita verkko-liittymät

Useita NIC voidaan liittää AM, mutta kun teet näin, huomioon seuraavasti:  

- AM koon tukee useita NIC. Jos haluat lisätietoja siitä, mitä AM koot tukevat useita NIC, [Windows Server AM koot](../virtual-machines/virtual-machines-windows-sizes.md) tai [Linux AM koot](../virtual-machines/virtual-machines-linux-sizes.md) artikkeleissa lukeminen   
- Vähintään kaksi NIC on luotava AM. Jos vain yksi NIC AM luodaan vaikka AM koon tukee useita, et voi liittää muita NIC AM sen luomisen jälkeen. Kun vähintään kaksi NIC AM on luotu, voit liittää muita NIC AM sen luomisen jälkeen, kunhan AM koon tukee enemmän kuin kaksi NIC.  
- Toissijainen NIC (ensisijainen NIC ei voi olla irrotettu) voivat katkaista AM Jos AM on vähintään kolme NIC liitetty. NIC ei voi irrottaa, jos AM on kaksi tai vähemmän NIC liitetään.  
- NIC sisällä AM järjestyksen satunnaisesti ja voi muuttua myös Azure infrastruktuurin päivitysten. Kuitenkin IP-osoitteet ja vastaavan ethernet MAC osoitteet, säilyvät ennallaan. Oletetaan, että käyttöjärjestelmä tunnistaa Azure NIC1 kuin Eth1. Eth1 on IP-osoite 10.1.0.100 ja 00-0D-3A-B0-39-0D MAC-osoite. Azure infrastruktuurin päivittää, ja Käynnistä tietokone uudelleen, kun käyttöjärjestelmä, jota voi nyt tunnistaa Azure NIC1 kuin Eth2, mutta IP- ja MAC-osoitteet on sama kuin siirretyt, kun käyttöjärjestelmä tunnistaa Azure NIC1 kuin Eth1. Jos uudelleenkäynnistys on asiakkaan käynnistämä, NIC-tilauksen pysyvät ennallaan käyttöjärjestelmässä.  
- Jos AM kuuluu [saatavuus](../azure-glossary-cloud-terminology.md#availability-set), kaikki VMs käytettävyys asettaa on oltava yhden NIC tai useita NIC. Jos VMs on useita NIC, numero kunkin heillä ei tarvitse olla sama, kunhan kunkin AM on vähintään kaksi NIC.

## <a name="next-steps"></a>Seuraavat vaiheet

- Opettele luomaan AM lukemalla artikkelin [Create AM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) yksittäisen NIC kanssa.
- Opettele luomaan AM lukemalla [käyttöönotto AM useita NIC kanssa](virtual-network-deploy-multinic-arm-ps.md) on artikkelissa useita NIC kanssa.
- Opettele luomaan NIC lukemalla [useita IP-osoitteiden Azuren näennäiskoneiden](virtual-network-multiple-ip-addresses-powershell.md) on artikkelissa useita IP-määrityksiä kanssa.
