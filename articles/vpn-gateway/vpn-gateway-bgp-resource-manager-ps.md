<properties
   pageTitle="Erityisen määrittämisestä Azure VPN yhdyskäytävien Azure Resurssienhallinta ja PowerShellin avulla | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi määrittäminen erityisen Azure VPN yhdyskäytävien Azure Resurssienhallinta ja PowerShellin avulla."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Kuinka voit määrittää erityisen Azure VPN yhdyskäytävien Azure Resurssienhallinta ja PowerShellin avulla

Tässä artikkelissa käydään läpi vaiheet ja paikallisen-sivusto (S2S) VPN-yhteyden ja resurssien hallinnan käyttöönottomalli ja PowerShell VNet VNet yhteyden erityisen käyttöön.


**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>Lisätietoja erityisen

Erityisen on vakio reititys-protokolla käytettyjä Internet Exchange reititys ja syy tietoja kahden tai useamman välillä. Erityisen mahdollistaa Azure VPN-yhdyskäytävien ja paikallisen VPN laitteilla, kutsutaan erityisen kollegat tai muiden tekijöiden, Exchange "reitittää", joka ilmoittaa sekä yhdyskäytävien saatavuus ja näiden etuliitteiden käsitellään yhdyskäytävien tai reitittimen yhdistävää syy. Erityisen voit myös ottaa salataanko siirrettävät reititys kesken useiden verkostojen mukaan välittäminen tiet erityisen yhdyskäytävän oppii yhden erityisen peer-kaikki erityisen kollegat.

Lisätietoja on hyötyä Lisää keskustelun erityisen ja ymmärrät tekniset vaatimukset ja huomioon otettavia seikkoja käytön erityisen [Yleiskatsaus on erityisen Azure VPN yhdyskäytävien kanssa](./vpn-gateway-bgp-overview.md) .

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Valitse Azure VPN yhdyskäytävien erityisen aloittaminen

Tässä artikkelissa opastaa vaihe vaiheelta, miten suorittaa seuraavia toimintoja:

- [Osa 1 – Ota käyttöön Azure VPN-yhdyskäytävän erityisen](#enablebgp)

- [Osa 2 – muodosta erityisen paikallisen-yhteys](#crossprembgp)

- [Osa 3 - erityisen VNet VNet yhteyden](#v2vbgp)

Jokaisen osan ohjeita muodostavat perus rakenneosan käyttöönoton erityisen verkkoyhteyden. Jos olet tehnyt kaikki kolme osaa, luot topologian, kuten seuraavassa kaaviossa on esitetty:

![Erityisen topologian](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Voit yhdistää ne yhteen, kun haluat luoda monimutkaisia, usean Toivottavasti pääset, salataanko siirrettävät verkkoon, joka vastaa tarpeitasi.

## <a name ="enablebgp"></a>Osa 1 – erityisen määrittämistä Azure VPN-yhdyskäytävän

Seuraavat määritysvaiheet asennuksen Azure VPN-yhdyskäytävän erityisen parametreja, kuten seuraavassa kaaviossa on esitetty:

![Erityisen yhdyskäytävän](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Ennen aloittamista

- Varmista, että sinulla on Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).
    
- Sinun on asennettava Azure Resurssienhallinta PowerShellin cmdlet-komennot. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.

### <a name="step-1---create-and-configure-vnet1"></a>Vaihe 1 – Luo ja määritä VNet1 

#### <a name="1-declare-your-variables"></a>1. määritellä oman muuttujat

Saat tämän Harjoitus asetetaan ensin ilmoittamalla Microsoftin muuttujat. Alla olevassa esimerkissä ilmoittaa arvot käytön tämän Harjoitus muuttujat. Varmista, arvojen korvaaminen omalla tuotannon määritettäessä. Voit käyttää muuttujia, jos käytössäsi on tämäntyyppinen kokoonpano tutustutaan vaiheet. Muokkaa muuttujaa, ja valitse kopioi ja liitä PowerShell console.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. muodostaa tilauksen ja luo uusi resurssiryhmä

Varmista, että voit siirtyä PowerShell-tilassa voit resurssien hallinnan Cmdlet-komentoja. Lisätietoja on artikkelissa [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md).

Avaa PowerShell-konsolin ja muodostaa yhteyden tiliisi. Seuraavassa esimerkissä avulla voit tarkastella:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. TestVNet1 luominen

Alla näyte Luo virtual verkon nimeltä TestVNet1 ja kolme aliverkosta, yksi nimeltä GatewaySubnet, yksi kutsuttu edusta ja yksi kutsuttu Taustajärjestelmä. Kun korvaat arvoja, on tärkeää nimi aina yhdyskäytävän aliverkon erityisesti GatewaySubnet. Jos nimeät sen jotain muuta, yhdyskäytävän luominen epäonnistuu. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Vaihe 2: Luo VPN-yhdyskäytävän TestVNet1 erityisen parametreilla

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. luominen IP-osoite ja aliverkon käyttömahdollisuudet

Pyydä julkinen IP-osoite, että VNet luodaan yhdyskäytävän jaetaan. Määrität myös aliverkon ja IP-määrityksiä pakollinen. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. Luo VPN-yhdyskäytävän numerolla AS

Luo TestVNet1 VPN-yhdyskäytävä. Huomaa, että erityisen vaativat reitti-pohjainen VPN-yhdyskäytävän ja lisäys-parametri - Asn, voit määrittää TestVNet1 ASN (AS luku). Luoda yhdyskäytävän voi viedä aikaa (vähintään 30 minuuttia suorittamiseen).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Hae Azure erityisen Peer IP-osoite

Kun yhdyskäytävä on luotu, sinun on Azure VPN-yhdyskäytävän erityisen Peer IP-osoitteen. Tätä osoitetta tarvitaan määrittää Azure VPN-yhdyskäytävän erityisen vertaisjärjestelmä paikallisen VPN-laitteisiin.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

Viimeisen komennon näyttää vastaavan erityisen käyttömahdollisuudet Azure VPN-yhdyskäytävän; Esimerkki:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Kun yhdyskäytävä on luotu, voit vahvistaa paikallisen- tai VNet VNet-yhteyden kanssa erityisen tämän yhdyskäytävän. Seuraavissa osissa käy läpi vaiheet käyttöoikeuden.

## <a name ="crossprembbgp"></a>Osa 2 – muodosta erityisen paikallisen-yhteys

Paikallisen-yhteyden muodostaminen haluat luoda paikallisen yhdyskäytävien edustavan paikallisen VPN-laite ja yhteyden Azure VPN-yhdyskäytävän paikalliseen verkkoon kirjauduttaessa Gatewayn. Tässä artikkelissa annettuja ohjeita välinen ero on määritettävä erityisen määritysten parametrit lisäominaisuuksia.

![Paikallisen-erityisen](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Ennen kuin jatkat, varmista, että olet suorittanut tämän harjoitus [osa 1](#enablebgp) .

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Vaihe 1 – Luo ja määritä paikalliseen verkkoon kirjauduttaessa yhdyskäytävän

#### <a name="1-declare-your-variables"></a>1. määritellä oman muuttujat

Tämä Harjoitus edelleen luoda kaavion määritykset. Varmista arvojen korvaaminen, jota haluat käyttää kokoonpanosi niistä.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Pari paikalliseen verkkoon kirjauduttaessa yhdyskäytävän parametrit koskevia huomioita:

- Paikallinen yhdyskäytävien voi olla samassa tai eri paikkaan ja resurssiryhmä VPN-yhdyskäytävä. Tässä esimerkissä näytetään niitä eri resurssin ryhmien eri sijainneissa.

- Haluat määritellä paikalliseen verkkoon kirjauduttaessa yhdyskäytävän pienin etuliite on erityisen Peer IP-osoitteelle VPN-laitteesi Host ()-isäntäosoite. Tässä tapauksessa /32 on "10.52.255.254/32" etuliite.

- Muistutukseksi sinun on käytettävä eri erityisen lähetysilmoitukset paikallisen verkkojen ja Azure VNet välillä. He ovat samat, jos haluat muuttaa VNet ASN paikallisen VPN-laite käyttämällä ASN jo vertaiskoneeseen muiden erityisen muiden tekijöiden kanssa.
    
Ennen kuin jatkat, varmista, että olet muodostanut edelleen tilauksen 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävän Site5

Muista Luo resurssiryhmän, jos se ei luoda, ennen kuin paikalliseen verkkoon kirjauduttaessa yhdyskäytävän luominen. Huomaa, että paikallinen yhdyskäytävien kaksi muut parametrit: Asn ja BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Vaihe 2: Yhdistä VNet yhdyskäytävän ja paikallisen yhdyskäytävien

#### <a name="1-get-the-two-gateways"></a>1. Hae kaksi yhdyskäytävät

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. TestVNet1 Site5 yhteyden luominen

Tässä vaiheessa luoda yhteys-TestVNet1 Site5. Sinun on määritettävä "-EnableBGP $True" käyttöön erityisen tätä yhteyttä varten. Kuten edellä kerrottiin, on mahdollista, että sama Azure VPN-yhdyskäytävän erityisen ja erityisen yhteydet. Ellei erityisen on käytössä connection-ominaisuutta, Azure kokoustyötilaa voi kyseisen yhteyden erityisen, vaikka erityisen parametrit on jo määritetty sekä yhdyskäytävät.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


Seuraavassa esimerkissä on esitetty kirjoittaa erityisen Tietolähdemääritykset-osasta paikallisen VPN laitteen koskevat tämän Harjoitus parametrit:

    - Site5 ASN: 65050
    - Site5 erityisen IP: 10.52.255.254
    - Etuliitteiden ilmoittaminen: (esimerkiksi) 10.51.0.0/16 ja 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet erityisen IP: 10.12.255.30
    - Staattisen reitin: lisätään 10.12.255.30/32, reitti nexthop parhaillaan VPN-tunneliliittymän laitteen kanssa
    - eBGP Multihop: "multihop"-vaihtoehto varmistaa eBGP on otettu käyttöön laitteen tarvittaessa

Muodostaa yhteyttä muutaman minuutin kuluttua ja erityisen peering istunnon käynnistyy, kun IP-yhteys on muodostettu.
 
## <a name ="v2vbgp"></a>Osa 3 - erityisen VNet VNet yhteyden

Tässä osassa Lisää erityisen, VNet VNet yhteyden seuraavassa kuvassa esitetyllä tavalla. 

![VNet VNet erityisen](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Yllä olevassa luettelossa edellisen vaiheen Jatka alla olevia ohjeita. Sinun on suoritettava [osa I](#enablebgp) Luo ja määritä TestVNet1 ja VPN-yhdyskäytävän ja erityisen. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Vaihe 1 – TestVNet2 ja VPN-yhdyskäytävän luominen

On tärkeää varmistaa, että uusi virtual verkon TestVNet2, IP-osoitetilaa ei ole päällekkäinen VNet alueiden avulla.

Tässä esimerkissä virtual verkkojen kuulut samaan ‑tilaukseen. Voit määrittää eri tilaukset; välisten yhteyksien VNet VNet Lisätietoja saat lisätietoja [VNet VNet yhteyden määrittäminen](./vpn-gateway-vnet-vnet-rm-ps.md) . Varmista, että voit lisätä "-EnableBgp $True" yhteyden ottaminen käyttöön erityisen luotaessa.

#### <a name="1-declare-your-variables"></a>1. määritellä oman muuttujat

Varmista arvojen korvaaminen, jota haluat käyttää kokoonpanosi niistä.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Luo uusi resurssiryhmä TestVNet2

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Luo VPN-yhdyskäytävän TestVNet2 erityisen parametreilla

Pyydä julkinen IP-osoite, että VNet luodaan yhdyskäytävän jaetaan. Määrität myös aliverkon ja IP-määrityksiä pakollinen. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

Luo VPN-yhdyskäytävän AS numeron. Huomaa, että voit on ohittaa oletusarvo ASN Azure VPN-yhdyskäytävät. Saat yhdistetyn VNets lähetysilmoitukset saa olla sama erityisen ja salataanko siirrettävät reititys otetaan käyttöön.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Vaihe 2: yhteyden TestVNet1 ja TestVNet2 yhdyskäytävät

Tässä esimerkissä sekä yhdyskäytävät ovat samassa tilaus. Voit suorittaa tämän vaiheen samassa PowerShell-istunnossa.

#### <a name="1-get-both-gateways"></a>1. Hae sekä yhdyskäytävät

Varmista, että kirjautuminen ja muodosta yhteys tilauksen 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. Luo molemmat yhteydet

Tässä vaiheessa luodaan TestVNet2 TestVNet1 yhteyden, ja yhteys-TestVNet2 TestVNet1 avulla.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Muista erityisen käyttöön molemmat yhteydet.

Kun näiden vaiheiden suorittamisen yhteys voidaan muodostaa muutaman minuutin kuluttua ja erityisen peering istunnon päivitetään kerran ylöspäin VNet VNet-yhteys on valmis.

Jos olet suorittanut tämän Harjoitus kolme osaa, on määritetty verkkotopologia alla kuvatulla tavalla:

![VNet VNet erityisen](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Kun yhteys on valmis, voit lisätä näennäiskoneiden virtual verkkoihin. Lisätietoja on kohdassa [Create Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ohjeita.

