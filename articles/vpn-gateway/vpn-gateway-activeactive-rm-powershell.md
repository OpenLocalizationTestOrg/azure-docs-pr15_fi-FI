<properties
   pageTitle="Aktiivinen aktiivinen S2S VPN-yhteydet määrittäminen Azure VPN yhdyskäytävien Azure Resurssienhallinta ja PowerShellin avulla | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi määrittäminen aktiivinen aktiivinen yhteydet Azure VPN yhdyskäytävien Azure Resurssienhallinta ja PowerShellin avulla."
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
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Määritä aktiivinen aktiivinen S2S VPN-yhteydet Azure VPN yhdyskäytävien Azure Resurssienhallinta ja PowerShellin avulla

Tässä artikkelissa käydään läpi vaiheet aktiivinen aktiivinen rajat paikallisen ja resurssien hallinnan käyttöönottomalli ja PowerShell VNet VNet yhteyksien luominen.


**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Tietoja erittäin käytettävissä olevien paikallisten yhteyksistä

Saavuttamiseksi suuren käytettävyyden paikallisen-ja VNet VNet connectivity kannattaa ottaa käyttöön useita VPN-yhdyskäytäviä ja laatimalla useita rinnakkaisia verkkojen ja Azure välisiä yhteyksiä. Katso, yhteysasetukset ja topologian [erittäin käytettävissä olevien paikallisen ja VNet VNet yhteydet](./vpn-gateway-highlyavailable.md) .

Tässä artikkelissa on ohjeita aktiivinen aktiivinen rajat paikallisen VPN-yhteyden ja kaksi virtual verkkojen välisen yhteyden aktiivinen aktiivinen määrittäminen:

- [Osa 1 - luominen ja määrittäminen Azure VPN-yhdyskäytävän aktiivinen aktiivinen-tilassa](#aagateway)

- [Osa 2 – muodostaa aktiivinen aktiivinen rajat paikallisen yhteyksiä](#aacrossprem)

- [Osa 3 - muodostaa aktiivinen aktiivinen VNet VNet yhteyksiä](#aav2v)

- [Osa 4 - päivitys aiemmin yhdyskäytävän aktiivinen aktiivinen ja aktiivinen valmius välillä](#aaupdate)

Voit yhdistää ne yhteen, kun haluat luoda monimutkaisia, erittäin käytettävissä verkkotopologia tarpeita vastaavan.

>[AZURE.IMPORTANT] Huomaa, että aktiivinen aktiivinen-tilassa toimii vain korkean SKU


## <a name ="aagateway"></a>Osa 1 – Luo ja määritä aktiivinen aktiivinen VPN yhdyskäytävät

Seuraavat vaiheet määritetään Azure VPN-yhdyskäytävän aktiivinen aktiivinen-tilassa. Aktiivinen aktiivinen ja aktiivinen valmius-yhdyskäytävät tärkeimmät erot:

- Sinun on luotava kaksi yhdyskäytävän IP-määrityksiä kahden julkisen IP-osoitteet
- EnableActiveActiveFeature-merkinnän on määrittäminen
- Yhdyskäytävän tuote on oltava korkean

Muut ominaisuudet ovat samat kuin aktiivinen aktiivinen yhdyskäytävät. 

### <a name="before-you-begin"></a>Ennen aloittamista

- Varmista, että sinulla on Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).
    
- Sinun on asennettava Azure Resurssienhallinta PowerShellin cmdlet-komennot. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.

### <a name="step-1---create-and-configure-vnet1"></a>Vaihe 1 – Luo ja määritä VNet1

#### <a name="1-declare-your-variables"></a>1. määritellä oman muuttujat

Saat tämän Harjoitus asetetaan ensin ilmoittamalla Microsoftin muuttujat. Alla olevassa esimerkissä ilmoittaa arvot käytön tämän Harjoitus muuttujat. Varmista, arvojen korvaaminen omalla tuotannon määritettäessä. Voit käyttää muuttujia, jos käytössäsi on tämäntyyppinen kokoonpano tutustutaan vaiheet. Muokkaa muuttujaa, ja valitse kopioi ja liitä PowerShell console.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
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
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

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

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Vaihe 2: Luo VPN-yhdyskäytävän TestVNet1 aktiivinen aktiivinen-tilassa

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. Voit luoda julkisen IP-osoitteet ja yhdyskäytävän IP-määritykset

Kahden julkisen IP-osoitteen jaetaan oman VNet luodaan yhdyskäytävän pyytäminen Määrität myös aliverkon ja IP-määrityksiä pakollinen. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Luo VPN-yhdyskäytävän aktiivinen aktiivinen määritys

Luo TestVNet1 VPN-yhdyskäytävä. Huomaa, että kaksi GatewayIpConfig merkintöjä, ja EnableActiveActiveFeature merkintä on määritetty. Aktiivinen aktiivinen tila vaatii reitti-pohjainen VPN-yhdyskäytävän, korkean tuote. Luoda yhdyskäytävän voi viedä aikaa (vähintään 30 minuuttia suorittamiseen).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. hankkia yhdyskäytävän julkiseen IP-osoitteet ja erityisen Peer IP-osoite

Kun yhdyskäytävä on luotu, sinun on Azure VPN-yhdyskäytävän erityisen Peer IP-osoitteen. Tätä osoitetta tarvitaan määrittää Azure VPN-yhdyskäytävän erityisen vertaisjärjestelmä paikallisen VPN-laitteisiin.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Näytä kaksi julkiseen IP-osoitteet varattu VPN-yhdyskäytävän ja niiden vastaavan erityisen Peer IP-osoitteiden yhdyskäytävän jokaiselle esiintymälle käyttää seuraavat cmdlet-komennot:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

Yhdyskäytäväesiintymästä koostuva järjestyksen julkiseen IP-osoitteet ja vastaavan erityisen Peering osoitteet ovat samat. Tässä esimerkissä yhdyskäytävän AM kanssa 40.112.190.5 julkiseen IP käyttää 10.12.255.4 erityisen Peering osoite ja 138.91.156.129 Gatewaylle käyttää 10.12.255.5. Nämä tiedot tarvitaan, kun määrität käytössä paikallisen VPN laitteet aktiivinen aktiivinen yhdyskäytävän muodostamisesta. Yhdyskäytävän näkyy näkyvän kaavion kaikki osoitteet:

![aktiivinen aktiivinen yhdyskäytävän](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Kun yhdyskäytävä on luotu, voit käyttää tätä yhdyskäytävän aktiivinen aktiivinen rajat paikallisen tai VNet VNet yhteyden muodostamisesta. Seuraavissa osissa käy läpi vaiheet käyttöoikeuden.


## <a name ="aacrossprem"></a>Osa 2 – aktiivinen aktiivinen rajat paikallisen yhteydet

Paikallisen-yhteyden muodostaminen haluat luoda paikallisen yhdyskäytävien edustavan paikallisen VPN-laite ja yhteyden Azure VPN-yhdyskäytävän paikalliseen verkkoon kirjauduttaessa Gatewayn. Tässä esimerkissä Azure VPN-yhdyskäytävä on aktiivinen aktiivinen-tilassa. Tuloksena on vain yksi paikallisen VPN-laite (paikallinen yhdyskäytävien) ja yksi yhteys resurssi, mutta sekä Azure VPN yhdyskäytäväesiintymästä koostuva yhteisten S2S VPN tunneleita paikallisen laitteen kanssa.

Ennen kuin jatkat, varmista, että olet suorittanut tämän harjoitus [osa 1](#aagateway) .

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Vaihe 1 – Luo ja määritä paikalliseen verkkoon kirjauduttaessa yhdyskäytävän

#### <a name="1-declare-your-variables"></a>1. määritellä oman muuttujat

Tämä Harjoitus edelleen luoda kaavion määritykset. Varmista arvojen korvaaminen, jota haluat käyttää kokoonpanosi niistä.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Pari paikalliseen verkkoon kirjauduttaessa yhdyskäytävän parametrit koskevia huomioita:

- Paikallinen yhdyskäytävien voi olla samassa tai eri paikkaan ja resurssiryhmä VPN-yhdyskäytävä. Tässä esimerkissä näytetään niitä eri resurssiryhmiä mutta Azure samassa sijainnissa.

- Jos näkyvissä on vain yksi paikallisen VPN-laite, yllä olevassa muodossa, aktiivinen aktiivinen yhteys käyttää kanssa tai ilman erityisen-protokollaa. Tässä esimerkissä erityisen paikallisen-yhteyden.

- Erityisen on käytössä, jos haluat määritellä paikalliseen verkkoon kirjauduttaessa yhdyskäytävän etuliite on erityisen Peer IP-osoitteelle VPN-laitteesi Host ()-isäntäosoite. Tässä tapauksessa /32 on "10.52.255.253/32" etuliite.

- Muistutukseksi sinun on käytettävä eri erityisen lähetysilmoitukset paikallisen verkkojen ja Azure VNet välillä. He ovat samat, jos haluat muuttaa VNet ASN, jos paikallisen VPN-laite on jo käytössä ASN vertaiskoneeseen muiden erityisen muiden tekijöiden kanssa.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävän Site5
    
Ennen kuin jatkat, varmista, että olet muodostanut edelleen tilauksen 1. Luo resurssiryhmän, jos se ei ole vielä luotu.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Vaihe 2: Yhdistä VNet yhdyskäytävän ja paikallisen yhdyskäytävien

#### <a name="1-get-the-two-gateways"></a>1. Hae kaksi yhdyskäytävät

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. TestVNet1 Site5 yhteyden luominen

Tässä vaiheessa luoda yhteys-TestVNet1 Site5_1 "EnableBGP" asettaminen $True kanssa.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN- ja erityisen parametrit paikallisen VPN-laitetta varten

Seuraavassa esimerkissä on esitetty kirjoittaa erityisen Tietolähdemääritykset-osasta paikallisen VPN laitteen koskevat tämän Harjoitus parametrit:

    - Site5 ASN: 65050
    - Site5 erityisen IP: 10.52.255.253
    - Etuliitteiden ilmoittaminen: (esimerkiksi) 10.51.0.0/16 ja 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet erityisen IP-osoite 1: 10.12.255.4 tunnelin 40.112.190.5
    - Azure VNet erityisen IP-osoite 2: 10.12.255.5 tunnelin 138.91.156.129
    - Staattinen tiet: kohde 10.12.255.4/32, nexthop VPN-tunnelin rajapinta 40.112.190.5 kohde 10.12.255.5/32 nexthop VPN-tunnelin rajapinta 138.91.156.129
    - eBGP Multihop: "multihop"-vaihtoehto varmistaa eBGP on otettu käyttöön laitteen tarvittaessa

Muodostaa yhteyttä muutaman minuutin kuluttua ja erityisen peering istunnon käynnistyy, kun IP-yhteys on muodostettu. Tässä esimerkissä on määritetty tähän mennessä vain yhden paikallisen VPN laitteella, jolloin kaavion alla:

![aktiivinen-aktiivinen-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Vaihe 3 – Yhdistä kaksi paikallisen VPN laitetta aktiivinen aktiivinen VPN-yhdyskäytävän

Jos sinulla on kaksi VPN-laitteiden osoitteessa samaan paikalliseen verkkoon, voit toteuttaa kahden redundancy yhdistämällä Azure VPN-yhdyskäytävän toisen VPN-laite.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. Luo toinen lähiverkon yhdyskäytävän Site5

Huomaa, että yhdyskäytävän IP-osoite, osoite etuliite ja erityisen peering osoite toisen paikalliseen verkkoon kirjauduttaessa yhdyskäytävän saa olla päällekkäiset edellisen paikalliseen verkkoon kirjauduttaessa Gatewayn, sama paikallista verkkoa varten. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. Yhdistä VNet yhdyskäytävän ja toinen lähiverkon yhdyskäytävän

Yhteyden luominen TestVNet1 Site5_2 kanssa "EnableBGP" $True asettaminen

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN ja erityisen toisen paikallisen VPN laitteen parametrit

Vastaavasti luetteloiden alapuolella parametrit haluat lisätä toisen VPN-laitteeseen:

    - Site5 ASN: 65050
    - Site5 erityisen IP: 10.52.255.254
    - Etuliitteiden ilmoittaminen: (esimerkiksi) 10.51.0.0/16 ja 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet erityisen IP-osoite 1: 10.12.255.4 tunnelin 40.112.190.5
    - Azure VNet erityisen IP-osoite 2: 10.12.255.5 tunnelin 138.91.156.129
    - Staattinen tiet: kohde 10.12.255.4/32, nexthop VPN-tunnelin rajapinta 40.112.190.5 kohde 10.12.255.5/32 nexthop VPN-tunnelin rajapinta 138.91.156.129
    - eBGP Multihop: "multihop"-vaihtoehto varmistaa eBGP on otettu käyttöön laitteen tarvittaessa

Kun (tunneleita)-yhteys on muodostettu, sinun on kaksi tarpeettomat VPN-laitteet ja yhteyden muodostaminen paikalliseen verkko- ja Azure tunneleita:

![kahden redundancy-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Osa 3 - aktiivinen aktiivinen VNet VNet yhteydet

Tässä osassa Luo yhteys aktiivinen aktiivinen VNet VNet erityisen. 

Yllä olevassa luettelossa edellisen vaiheen Jatka alla olevia ohjeita. Luo ja määritä TestVNet1 ja VPN-yhdyskäytävän ja erityisen [osa 1](#aagateway) on suoritettu. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Vaihe 1 – TestVNet2 ja VPN-yhdyskäytävän luominen

On tärkeää varmistaa, että uusi virtual verkon TestVNet2, IP-osoitetilaa ei ole päällekkäinen VNet alueiden avulla.

Tässä esimerkissä virtual verkkojen kuulut samaan ‑tilaukseen. Voit määrittää eri tilaukset; välisten yhteyksien VNet VNet Lisätietoja saat lisätietoja [VNet VNet yhteyden määrittäminen](./vpn-gateway-vnet-vnet-rm-ps.md) . Varmista, että voit lisätä "-EnableBgp $True" yhteyden ottaminen käyttöön erityisen luotaessa.

#### <a name="1-declare-your-variables"></a>1. määritellä oman muuttujat

Varmista arvojen korvaaminen, jota haluat käyttää kokoonpanosi niistä.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
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
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Luo uusi resurssiryhmä TestVNet2

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. Luo aktiivinen aktiivinen VPN-yhdyskäytävän TestVNet2

Kahden julkisen IP-osoitteen jaetaan oman VNet luodaan yhdyskäytävän pyytäminen Määrität myös aliverkon ja IP-määrityksiä pakollinen. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Luo VPN-yhdyskäytävän liitetään numero ja "EnableActiveActiveFeature"-merkinnän. Huomaa, että voit on ohittaa oletusarvo ASN Azure VPN-yhdyskäytävät. Saat yhdistetyn VNets lähetysilmoitukset saa olla sama erityisen ja salataanko siirrettävät reititys otetaan käyttöön.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Vaihe 2: yhteyden TestVNet1 ja TestVNet2 yhdyskäytävät

Tässä esimerkissä sekä yhdyskäytävät ovat samassa tilaus. Voit suorittaa tämän vaiheen samassa PowerShell-istunnossa.

#### <a name="1-get-both-gateways"></a>1. Hae sekä yhdyskäytävät

Varmista, että, kirjaudu sisään ja muodosta yhteys tilauksen 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. Luo molemmat yhteydet

Tässä vaiheessa luodaan TestVNet2 TestVNet1 yhteyden, ja yhteys-TestVNet2 TestVNet1 avulla.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Muista erityisen käyttöön molemmat yhteydet.

Yhteyden muodostavat perheelle muutama minuutti ja erityisen näiden vaiheiden suorittamisen jälkeen peering istunnon tulee määrittäminen, kun VNet VNet yhteyden muodostamista kahden vikasietoisuusominaisuuksilla:

![aktiivinen-aktiivinen-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Osa 4 - päivitys aiemmin yhdyskäytävän aktiivinen aktiivinen ja aktiivinen valmius välillä

Viimeisessä osassa kerrotaan, miten voit määrittää aiemmin Azure VPN-yhdyskäytävän aktiivinen valmius aktiivinen aktiivinen tilaan tai päinvastoin.

>[AZURE.IMPORTANT] Huomaa, että aktiivinen aktiivinen-tilassa toimii vain korkean SKU

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Määritä aktiivinen valmius-gateway aktiivinen aktiivinen yhdyskäytävään

#### <a name="1-gateway-parameters"></a>1. parametrien yhdyskäytävän

Seuraavassa esimerkissä muuntaa aktiivinen valmius-yhdyskäytävän aktiivinen aktiivinen-yhdyskäytävä. Haluat luoda toisen julkiseen IP-osoite ja valitse Lisää toinen yhdyskäytävän IP-määritys. Alla on esitetty parametrit, joita käytetään:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. Luo julkinen IP-osoite ja valitse Lisää toinen yhdyskäytävän IP-määritys

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. aktiivinen aktiivinen-tila käyttöön ja päivittää yhdyskäytävän

Yhdyskäytävän objekti on määritettävä PowerShell käynnistettävän todellinen päivitys. Yhdyskäytävän objektin SKU myös on vaihdettava korkean, koska se on luotu aiemmin vakioasetuksena.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Päivitys voi kestää 30-45 minuuttia.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Aktiivinen valmius yhdyskäytävään aktiivinen aktiivinen yhdyskäytävän asetusten määrittäminen

#### <a name="1-gateway-parameters"></a>1. parametrien yhdyskäytävän

Käytä samoja parametreja kuin yllä, IP-määritys, jonka haluat poistaa nimen.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. poistaa yhdyskäytävän IP-määritys ja aktiivinen aktiivinen-tilan poistaminen käytöstä

Vastaavasti sinun on määritettävä yhdyskäytävän objektin PowerShellin käynnistettävän todellinen päivitys.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Päivitys voi kestää jopa 30 45 minuuttia.


## <a name="next-steps"></a>Seuraavat vaiheet

Kun yhteys on valmis, voit lisätä näennäiskoneiden virtual verkkoihin. Lisätietoja on kohdassa [Create Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ohjeita.

