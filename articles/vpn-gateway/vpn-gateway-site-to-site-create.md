<properties
   pageTitle="Luo virtuaalisia verkko Azure perinteinen portaalissa sivusto sivusto VPN-yhdyskäytävän yhteys | Microsoft Azure"
   description="Luo VNet paikallisen-S2S VPN-yhdyskäytävän yhteyden ja perinteinen käyttöönoton mallin yhdistelmäympäristö."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Luo VNet sivuston sivuston yhteyden Azure perinteinen-portaalissa

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Perinteinen - perinteinen Portal](vpn-gateway-site-to-site-create.md)

Tässä artikkelissa käydään läpi luominen virtual verkko- ja sivusto sivusto VPN yhdyskäytävän yhteys paikalliseen verkkoon perinteinen käyttöönottomalli ja perinteinen portaalin. Sivuston sivuston yhteydet voi käyttää rajat paikallinen tai yhdistelmäympäristössä määrityksiä.

![Sivusto kaavio] (./media/vpn-gateway-site-to-site-create/site2site.png "sivuston sivusto")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien sivuston sivuston yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Seuraavassa taulukossa on tällä hetkellä käytettävissä käyttöönoton mallit ja viestintämenetelmien sivusto määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Lisämäärityksiä 

Jos haluat yhdistää VNets toisiinsa, katso [VNet VNet yhteyden perinteinen käyttöönoton mallin määrittäminen](virtual-networks-configure-vnet-to-vnet-connection.md). Jos haluat lisätä sivuston sivuston yhteyden VNet, jossa on jo yhteyden, on [Lisää VNet aiemmin VPN-yhdyskäytävän yhteys S2S yhteyden](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Ennen aloittamista

Tarkista, pidä seuraavat asiat ennen aloittamista määritys.

- Yhteensopivat VPN-laite ja henkilölle, joka ei voi määrittää sen. Lisätietoja on artikkelissa [VPN-laitteiden tietoja](vpn-gateway-about-vpn-devices.md). Vaikka et olisikaan tuttuja määrittäminen VPN-laite- tai tunne alueiden sijaitsevat IP-osoite paikallisen oman verkon määritysten, sinulla on oltava henkilön kanssa, jolla voit antaa näiden tietojen järjestämiseen.

- Ulkoisesti aukeaman julkiseen IP-osoite VPN-laitteeseesi. Tämä IP-osoite ei löydy, takana tulkintatoiminnon

- Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).


## <a name="CreateVNet"></a>Luo virtuaalisia verkossa

1. Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com/).

2. Valitse näytön vasemmassa alakulmassa **Uusi**. Valitse siirtymisruudussa **Verkkopalveluita**ja valitse sitten **VPN**. Valitse Aloita ohjattu määritystoiminto **Mukautettu luominen** .

3. Jos haluat luoda oman VNet, kirjoita määritysasetukset seuraavilla sivuilla:

## <a name="Details"></a>VPN-tiedot-sivu

Anna seuraavat tiedot:

- **Nimi**: virtual verkon nimi. Esimerkiksi *EastUSVNet*. Käytät tätä VPN-nimeä, kun otat käyttöön VMs ja PaaS esiintymät, joten et ehkä halua tehdä liian monimutkaiselta nimi.
- **Sijainti**: sijainti suoraan liittyvät resurssit (VMs) sijaitsevan haluamaasi fyysinen sijainti (alue). Esimerkiksi jos haluat VMs, sijoitettava fyysisesti *Yhdysvaltojen Itä*virtual verkoston käyttöön, valitse kyseiseen sijaintiin. Et voi muuttaa alueen liittyvät virtual verkon sen luomisen jälkeen.

## <a name="DNS"></a>DNS-palvelimet ja VPN-yhteyden sivu

Seuraavat tiedot ja valitse sitten oikeassa alakulmassa seuraava-nuoli.

- **DNS-palvelimet**: DNS-palvelimen nimi ja IP-osoite tai valitse aiemmin rekisteröidyn DNS-palvelin pikavalikosta. Tämä asetus luo DNS-palvelimeen. Sen avulla voit määrittää DNS-palvelimet, jota haluat käyttää virtual verkoston nimenselvitys.
- **Määritä sivusto sivusto VPN**: kohdassa **määrittäminen sivuston sivusto-VPN**valintaruutu.
- **Paikalliseen verkkoon kirjauduttaessa**: lähiverkossa edustaa fyysinen paikallisen sijainti. Voit valita lähiverkossa aiemmin luomasi tai voit luoda uuden paikalliseen verkkoon. Jos valitset Käytä lähiverkossa aiemmin luomasi, siirry **Paikallisen verkkojen** määritys-sivulla ja täsmällisen VPN-laite VPN laitteen IP-osoite (julkinen aukeaman IPv4-osoite).

## <a name="Connectivity"></a>Sivuston sivuston yhdistämispalvelun sivulla

Jos luot uuden paikalliseen verkkoon, näet **Sivuston sivuston yhdistämispalvelun** sivulla. Jos haluat käyttää aiemmin luomasi paikalliseen verkkoon, ohjattu toiminto ei näy tällä sivulla ja voit siirtää seuraavaan osaan.

Seuraavat tiedot ja valitse sitten Seuraava-nuoli.

-   **Nimi**: nimi, jolle haluat soittaa paikallisesta (paikallisen) verkko-sivustossa.
-   **VPN-laitteen IP-osoite**: paikallisen VPN laite, jonka avulla Azure yhdistäminen julkisen aukeaman IPv4-osoite. VPN-laite ei löydy, takana tulkintatoiminnon
-   **Osoitetilaa varten**: Sisällytä käynnistäminen IP-osoite ja CIDR (osoite määrä). Voit määrittää osoitteen range(s), jonka haluat lähettää paikallisen paikalliseen sijaintiin VPN-yhdyskäytävän kautta. Jos kohde-IP-osoite kuuluu alueet, jotka voit määrittää, se siirretään VPN-yhdyskäytävän kautta.
-   **Lisää osoitetilaa**: Jos käytössäsi on useita osoitealueita, jonka haluat lähettää VPN-yhdyskäytävän kautta, määritä osoitteen kunkin alueen. Voit lisätä tai poistaa alueita myöhemmin **Lähiverkossa** -sivulla.

## <a name="Address"></a>VPN osoite välilyöntejä-sivu

Määritä osoitealue, jota haluat käyttää virtual verkkoa varten. Nämä ovat dynaamisia IP-osoitteet (tapahtuneen), joka määritetään VMs ja muut rooli esiintymät, jotka virtual verkoston käyttöön.

On tärkeää erityisesti alue, joka ei ole päällekkäinen avulla alueet, joita käytetään paikallista verkkoa varten. Sinun täytyy verkonvalvoja sointuvat. Verkonvalvoja on ehkä carve ulos alueen IP-osoitteiden paikallisen verkon osoite tilasta voit käyttää virtual verkkoa varten.

Seuraavat tiedot ja valitse sitten oikeassa alakulmassa verkon määrittäminen valintamerkki.

- **Osoitetilaa varten**: Sisällytä käynnistäminen IP-osoite ja osoitteiden määrä. Varmista, että määrität osoite välilyöntejä ole päällekkäin jonkin osoite välilyöntejä, joihin sinulla on paikallisen verkossa.
- **Lisää aliverkon**: Sisällytä käynnistäminen IP- ja osoitteiden määrä. Lisää aliverkosta eivät ole pakollisia, mutta voit halutessasi luoda erilliset aliverkon VMs, joka on staattinen tapahtuneen. Tai voit halutessasi oman VMs aliverkon, joka on erillään muiden roolin esiintymien.
- **Lisää yhdyskäytävän aliverkon**: Lisää yhdyskäytävän aliverkon. Yhdyskäytävän aliverkon käytetään vain VPN-yhdyskäytävän ja määritysten tarvitaan.

Valitse sivun alareunassa valintamerkki ja virtual verkon alkaa luomiseen. Kun se on valmis, näet **luotu** kohdasta **tila** **verkot** -sivulla Azure perinteinen-portaalissa. Kun VNet on luotu, voit määrittää VPN-yhdyskäytävä.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>Määritä VPN-gateway

Määritä suojattu sivusto yhteyden luominen VPN-yhdyskäytävä. Lisätietoja on kohdassa [Configure virtual yhdyskäytävien Azure perinteinen-portaalissa](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Kun yhteys on valmis, voit lisätä näennäiskoneiden virtual verkkoihin. Lisätietoja on [näennäiskoneiden](https://azure.microsoft.com/documentation/services/virtual-machines/) ohjeissa.
