<properties
   pageTitle="Useita VPN-sivusto sivusto yhdyskäytävän yhteyksiä lisääminen virtual verkon Azure-portaalissa Resurssienhallinta käyttöönoton mallin | Microsoft Azure"
   description="Usean sivuston S2S yhteyksien lisääminen VPN-yhdyskäytävän, jossa on olemassa oleva yhteys"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Sivusto yhteyden lisääminen VNet aiemmin luodun VPN-yhdyskäytävä-yhteyden avulla

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Perinteinen - PowerShell](vpn-gateway-multi-site.md)

Tässä artikkelissa käydään läpi sivusto (S2S)-yhteyksien lisääminen, jossa on aiemmin luotu yhteys VPN-yhdyskäytävän Azure portaalin avulla. Tämä yhteystyyppi kutsutaan usein "usean sivuston"-määritys. 

Tämän artikkelin avulla voit lisätä S2S yhteyden VNet, jossa on jo S2S yhteyden, pisteen sivuston yhteyden tai VNet VNet yhteyden. Liittyy tiettyjä rajoituksia, kun lisäät yhteydet. Tarkista [ennen aloittamista](#before) -osassa voit tarkistaa ennen kuin aloitat kokoonpanosi tämän artikkelin. 

Tämä artikkeli koskee resurssien hallinnan käyttöönottomalli-sovelluksella luodut VNets, joissa on RouteBased VPN-yhdyskäytävä. Nämä vaiheet eivät vaikuta ExpressRoute /-,-sivusto samanaikaisesti olemassa olevien yhteyden määrityksiä. Saat lisätietoja samanaikaisesti olemassa olevien yhteyksien [ExpressRoute/S2S samanaikaisesti olemassa olevien yhteydet](../expressroute/expressroute-howto-coexist-resource-manager.md) .

### <a name="deployment-models-and-methods"></a>Käyttöönotto-malleja ja menetelmät

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Päivitämme tämän taulukon kuin uusia artikkeleita ja muita työkaluja palvelupakettiisi määritysten. Artikkelin ollessa käytettävissä on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Ennen aloittamista

Tarkista seuraavat seikat:

- Olet luomassa ei samanaikaisesti olemassa olevien ExpressRoute/S2S-yhteys.
- Sinulla on virtual verkko, joka on luotu käyttämällä aiemmin luotua yhteyttä resurssien hallinnan käyttöönottomalli.
- Oman VNet virtual yhdyskäytävien on RouteBased. Jos sinulla on PolicyBased VPN-yhdyskäytävän, VPN-yhdyskäytävän poisto ja luo uusi VPN-yhdyskäytävän RoutBased nimellä.
- Ei mitään osoitealueita päällekkäin minkään VNets, tämä VNet muodostaa yhteyden.
- Sinulla on yhteensopiva VPN-laite ja henkilölle, joka ei voi määrittää sen. Lisätietoja on artikkelissa [VPN-laitteiden tietoja](vpn-gateway-about-vpn-devices.md). Vaikka et olisikaan tuttuja määrittäminen VPN-laite- tai tunne alueiden sijaitsevat IP-osoite paikallisen oman verkon määritysten, sinulla on oltava henkilön kanssa, jolla voit antaa näiden tietojen järjestämiseen.
- Ulkoisesti aukeaman julkiseen IP-osoite on VPN-laitteeseesi. Tämä IP-osoite ei löydy, takana tulkintatoiminnon


## <a name="part1"></a>Osa 1 - yhteyden määrittäminen

1. Selaimessa, siirry [Azure portal](http://portal.azure.com) ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse **kaikki resurssit** käyttämäsi **VPN-yhdyskäytävän** resurssit-luettelosta ja napsauttamalla sitä.
3. Valitse **yhteydet** **Virtual verkkoyhdyskäytävä** -sivu.

    ![Yhteydet-sivu](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Valitse **+ Lisää** **yhteydet** -sivu.

    ![Lisää yhteys-painike](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. Valitse **Lisää yhteys** -sivu Täytä seuraavat kentät:
    - **Nimi:** Nimi, jonka haluat antaa sivuston, yhteys luodaan.
    - **Yhteyden tyyppi:** Valitse **sivusto (IP)**.

    ![Lisää yhteys-sivu](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Osa 2 - Lisää paikallisen yhdyskäytävien

1. Valitse **Paikallinen yhdyskäytävien** ***Valitse paikallinen yhdyskäytävien***. **Valitse lähiverkon yhdyskäytävä** -sivu avautuu.

    ![Valitse paikallinen yhdyskäytävien](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Valitse **Luo uusi** Avaa **Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävä** -sivu.

    ![Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävä-sivu](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. **Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävä** -sivu, Täytä seuraavat kentät:
    - **Nimi:** Haluat antaa paikallisen verkkoresurssin yhdyskäytävän nimi.
    - **IP-osoite:** Julkinen sivusto, johon haluat muodostaa VPN-laitteen IP-osoite.
    - **Osoite tila:** Osoitetilaa, jonka haluat reitittää sivustoon paikalliseen verkkoon kirjauduttaessa.
4. Valitse **Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävä** -sivu, Tallenna muutokset valitsemalla **OK** .

## <a name="part3"></a>Osa 3 - jaetun avaimen lisääminen ja yhteyden luominen

1. Lisää **Lisää yhteys** -sivu jaetun avaimen, jota haluat käyttää yhteyden luomiseen. Voit saada jaetun avaimen VPN-laitteen tai määrittää tähän ylöspäin ja määrittämällä sitten VPN-laitteen käyttäminen saman jaetun avaimen. Tärkeintä on näppäimet ovat täsmälleen samat.

    ![Jaettu avain](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Alareunassa sivu valitsemalla **OK** yhteyden luominen.

## <a name="part4"></a>Osa 4 - Tarkista VPN-yhteyden

Voit tarkistaa VPN-yhteyden portaalissa tai PowerShell-toiminnolla.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Seuraavat vaiheet

- Kun yhteys on valmis, voit lisätä näennäiskoneiden virtual verkkoihin. Katso näennäiskoneiden [oppimispolku](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) lisätietoja.
