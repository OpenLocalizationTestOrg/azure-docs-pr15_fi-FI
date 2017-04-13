<properties
   pageTitle="Yhdistä käyttäen resurssien hallinnan käyttöönottomalli ja Azure-portaalin VNets | Microsoft Azure"
   description="Luo VPN yhdyskäytävän yhteys VNets käyttämällä Resurssienhallinta ja Azure-portaalin välillä."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Yhteysasetusten VNet VNet Azure-portaalissa

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Perinteinen - perinteinen Portal](virtual-networks-configure-vnet-to-vnet-connection.md)


Tässä artikkelissa käydään läpi vaiheet ja luoda VNets Resurssienhallinta käyttöönoton mallin avulla VPN-yhdyskäytävän ja Azure-portaalin välinen yhteys.

Kun muodostat yhteyden virtual verkkojen Azure portal-VNets on oltava sama tilaus. Jos virtual lopettaminen ovat eri tilaukset, voit yhdistää ne käyttämällä [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) -ohjeita.

![V2V kaavio](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien VNet VNet yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Seuraavassa taulukossa on tällä hetkellä käytettävissä käyttöönoton mallit ja viestintämenetelmien VNet VNet määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Tietoja VNet VNet yhteyksistä

Yhteyden muodostaminen toiseen virtual virtual verkon (VNet VNet) on samanlainen kuin yhdistäminen VNet paikallisen sivuston sijainnissa. Yhteyksien molempien tietotyyppien käyttää Azure VPN-yhdyskäytävän suojatun tunnelin IP/IKE. Voit yhdistää VNets voi olla eri alueille tai eri tilaukset.

Voit myös yhdistää VNet VNet tietoliikenteen usean sivuston määrityksiä. Näillä oikeuksilla muodostat rajat verkkotopologioita, joka yhdistää paikallista connectivity väliset VPN-yhteyttä, kuten seuraavassa kaaviossa on esitetty:

![Tietoja yhteyksistä] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Tietoja yhteyksistä")

### <a name="why-connect-virtual-networks"></a>Miksi yhteyden virtual verkkoja?

Voit yhdistää virtual verkkojen seuraavista syistä:

- **Toimintojen välinen alueen geo-redundanssin ja geo tavoitettavuus**
    - Voit määrittää oman geo replikoinnin tai synkronoinnin suojattua yhteyttä siirtymättä Internetiin yhteydessä oleva päätepisteet päälle.
    - Azure liikenteen hallinta ja kuormituksen voit määrittää erittäin käytettävissä kuormituksen vikasietoisuusominaisuuksilla geo useita Azure alueiden välillä. Tärkeää esimerkki on määrittäminen SQL aina käyttöön käytettävyys ryhmiin levittäminen useita Azure alueiden välillä.

- **Alueellisten monitasoisten sovellusten eristystaso tai järjestelmänvalvojan reunaa**
    - Samalla alueella voit määrittää monitasoisten sovellusten useita virtual verkkoja yhdistää vuoksi eristystaso tai järjestelmänvalvojan vaatimusten kanssa.

Saat lisätietoja VNet VNet yhteydet [VNet VNet usein kysytyt kysymykset](#faq) on tämän artikkelin lopussa.

### <a name="values"></a>Esimerkki asetukset

Käytä näitä vaiheita hioa, voit määritysten esimerkkiarvojen. Esimerkki tarkoituksiin Käytämme useita osoite välilyöntejä kunkin VNet varten. Kuitenkin VNet VNet määrityksiä ei edellytä useita osoite välilyöntejä.

**TestVNet1 arvot:**

- VNet nimi: TestVNet1
- Tilaa osoite: 10.11.0.0/16
    - Aliverkon nimi: FrontEnd
    - Aliverkon osoitealueita: 10.11.0.0/24
- Resurssiryhmä: TestRG1
- Sijainti: Itä US
- Tilaa osoite: 10.12.0.0/16
    - Aliverkon nimi: taustaan
    - Aliverkon osoitealueita: 10.12.0.0/24
- Yhdyskäytävän aliverkon nimi: GatewaySubnet (tämä on automaattinen täyttö-portaalissa)
    - Yhdyskäytävän aliverkon osoitealueita: 10.11.255.0/27
- DNS-palvelin: Käytä DNS-palvelimen IP-osoite
- VPN yhdyskäytävän nimi: TestVNet1GW
- Yhdyskäytävän tyyppi: VPN
- VPN-tyyppi: reitin perusteella
- TUOTE: Valitse yhdyskäytävän tuote, jota haluat käyttää
- Julkinen IP-osoitenimi: TestVNet1GWIP
- Yhteyden arvot:
    - Nimi: TestVNet1toTestVNet4
    - Jaetun avaimen: Voit luoda jaetun avaimen itse. Tässä esimerkissä käytetään abc123. Tärkeintä on, että VNets välinen yhteys luodessasi arvon vastattava.

**TestVNet4 arvot:**

- VNet nimi: TestVNet4
- Tilaa osoite: 10.41.0.0/16
    - Aliverkon nimi: FrontEnd
    - Aliverkon osoitealueita: 10.41.0.0/24
- Resurssiryhmä: TestRG1
- Sijainti: Länsi USA
- Tilaa osoite: 10.42.0.0/16
    - Aliverkon nimi: taustaan
    - Aliverkon osoitealueita: 10.42.0.0/24
- GatewaySubnet nimi: GatewaySubnet (tämä on automaattinen täyttö-portaalissa)
    - GatewaySubnet osoitealueita: 10.41.255.0/27
- DNS-palvelin: Käytä DNS-palvelimen IP-osoite
- VPN yhdyskäytävän nimi: TestVNet4GW
- Yhdyskäytävän tyyppi: VPN
- VPN-tyyppi: reitin perusteella
- TUOTE: Valitse yhdyskäytävän tuote, jota haluat käyttää
- Julkinen IP-osoitenimi: TestVNet4GWIP
- Yhteyden arvot:
    - Nimi: TestVNet4toTestVNet1
    - Jaetun avaimen: Voit luoda jaetun avaimen itse. Tässä esimerkissä käytetään abc123. Tärkeintä on, että VNets välinen yhteys luodessasi arvon vastattava.


## <a name="CreatVNet"></a>1. luominen ja määrittäminen TestVNet1

Jos sinulla on jo VNet, varmista, että asetukset ovat yhteensopivia VPN-yhdyskäytävän rakenne. Kiinnitä erityistä huomiota aliverkosta, jotka voivat olla muiden verkkojen. Jos sinulla on päällekkäisiä aliverkosta, yhteys ei toimi oikein. Jos yhteyttä VNet on määritetty oikein asetukset, voit aloittaa [Määritä DNS-palvelin](#dns) -osion ohjeita.

### <a name="to-create-a-virtual-network"></a>Virtuaalinen verkoston luominen

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. Lisää muita osoitetilaa varten ja luo aliverkosta

Voit lisätä muita osoitetilaa varten ja luo aliverkosta, kun yhteyttä VNet on luotu.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. yhdyskäytävän aliverkon luominen

Ennen yhteyden muodostamista virtual verkon yhdyskäytävän, sinun on luoda yhdyskäytävän aliverkon, johon haluat muodostaa virtual verkossa. Jos mahdollista kannattaa luoda yhdyskäytävän aliverkon käyttämän CIDR joukon /28 tai /27 sopimaan tulevien lisämäärityksiä vaatimukset tarpeeksi IP-osoitteet.

Jos luot määritysten hioa nimellä, tarkista näitä [Esimerkkejä asetuksista](#values) yhdyskäytävän aliverkon luotaessa.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Voit luoda yhdyskäytävän aliverkon

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. (valinnainen) DNS-palvelimen määrittäminen

Voit halutessasi nimenselvitys, jotka on otettu oman VNets näennäiskoneiden on määritettävä DNS-palvelimeen.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. VPN Gatewayn luominen

Tässä vaiheessa voit luoda virtuaalisen yhdyskäytävien oman VNet. Tämä vaihe voi kestää jopa 45 minuuttia. Jos olet luomassa määritysten kuin hioa, voit viitata [Esimerkki asetukset](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Voit luoda virtuaalisen yhdyskäytävien

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. luominen ja määrittäminen TestVNet4

Kun olet määrittänyt TestVNet1, luoda TestVNet4 toistamalla edelliset vaiheet-arvojen korvaaminen niiden TestVNet4. Ei tarvitse odottaa TestVNet1 virtual yhdyskäytävien luominen ennen TestVNet4 määrittäminen on valmis. Jos käytössäsi on oma arvoja, varmista, että osoite välilyöntejä ole päällekkäin VNets, johon haluat muodostaa yhteyden avulla.


## <a name="TestVNet1Connection"></a>7. TestVNet1-yhteyden määrittäminen

Kun TestVNet1 ja TestVNet4 VPN-yhdyskäytäviä, voit luoda virtuaalisen verkon yhdyskäytävän yhteydet. Tässä osassa muodostamalla yhteyden VNet1-VNet4.

1. **Kaikki resurssit**Siirry VPN-yhdyskäytävän oman VNet varten. Esimerkiksi **TestVNet1GW**. Valitse **TestVNet1GW** Avaa VPN-yhdyskäytävä-sivu.

    ![Yhteydet-sivu] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Yhteydet-sivu")

2. Valitse **+ Lisää** Avaa **Lisää yhteys** -sivu.

3. Kirjoita yhteyden nimi **Lisää yhteys** -sivu nimi-kenttään. Esimerkiksi **TestVNet1toTestVNet4**.

    ![Yhteyden nimi] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Yhteyden nimi")

4. **Yhteyden tyyppi**. Valitse avattavasta valikosta **VNet VNet** .

5. **Ensimmäinen VPN-yhdyskäytävä** -kenttäarvo täytetään automaattisesti, koska luot yhteyden määritetyn virtual yhdyskäytävien.

6. **Toinen VPN-yhdyskäytävä** -kenttä on VNet, jonka haluat muodostaa yhteyden, VPN-yhdyskäytävä. Valitse Avaa **Valitse VPN-yhdyskäytävä** -sivu **valitsemalla toisen VPN-yhdyskäytävä** .

    ![Lisää yhteys] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Yhteyden lisääminen")

7. Näytä tämä sivu VPN-yhdyskäytäviä, jotka on lueteltu. Huomaa, että vain VPN-yhdyskäytäviä, jotka ovat tilauksen näkyvät. Jos haluat muodostaa VPN-yhdyskäytävään, joka ei ole tilauksen, käytä [PowerShell-artikkelissa](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Valitse, johon haluat muodostaa VPN-yhdyskäytävä.
 
9. Kirjoita jaetun avaimen yhteyden **jaettu avain** -kenttään. Voit luoda tai Luo avain. Valitse sivusto yhteydet-käytettävän avaimen olisi täsmälleen samalla tavalla paikalliseen laitteen yhdyskäytävän VPN-yhteys. Joka muistuttaa, paitsi sijasta muodostaa VPN-laitteeseen, yrität muodostaa yhteyden toiseen VPN-yhdyskäytävä.

    ![Jaettu avain] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Jaettu avain")

10. Valitse **OK** sivu alalaidassa Tallenna muutokset.

## <a name="TestVNet4Connection"></a>8. TestVNet4-yhteyden määrittäminen

Luo seuraavaksi yhteys TestVNet4 TestVNet1. Käyttää samaa menetelmää, jota käytit yhteyden luominen TestVNet1 TestVNet4. Varmista, että käytät saman jaetun avaimen.

## <a name="VerifyConnection"></a>9. Tarkista yhteys

Varmista yhteys. Kunkin VPN-yhdyskäytävää toimi seuraavasti:

1. Etsi sivu VPN-yhdyskäytävän. Esimerkiksi **TestVNet4GW**. 
2. Valitse VPN-yhdyskäytävä-sivu **yhteydet** , jolloin näkyviin VPN-yhdyskäytävän yhteydet-sivu.

Tarkastele yhteydet ja Tarkista tila. Kun yhteys on luotu, näet **onnistui** ja **yhdistetty** muodossa tila-arvoja.

![Onnistui] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Onnistui")

Voit kaksoisnapsauttaa kunkin yhteyden erikseen, jos haluat tarkastella lisätietoja yhteys.

![Olennaiset asiat] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Olennaiset asiat")

## <a name="faq"></a>VNet VNet usein kysytyt kysymykset

Saat lisätietoja VNet VNet yhteydet tarkastella usein kysytyt kysymykset.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Kun yhteys on valmis, voit lisätä näennäiskoneiden virtual verkkoihin. Lisätietoja on kohdassa [Create Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ohjeita.
