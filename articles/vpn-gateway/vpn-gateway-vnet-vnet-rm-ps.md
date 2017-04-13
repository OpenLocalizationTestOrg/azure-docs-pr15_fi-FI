<properties
   pageTitle="Yhteyttä VPN-yhdyskäytävän ja PowerShellin Azure VNets | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi virtual verkostojen yhdistäminen toisiinsa Azure Resurssienhallinta ja PowerShellin avulla."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Yhteysasetusten VNet VNet for resurssien hallinta PowerShellin avulla

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Perinteinen - perinteinen Portal](virtual-networks-configure-vnet-to-vnet-connection.md)

Tässä artikkelissa käydään läpi vaiheet ja luoda VNets Resurssienhallinta käyttöönoton mallin avulla VPN-yhdyskäytävän välinen yhteys. Virtuaalinen verkot voivat olla samassa tai eri alueilla ja saman tai toisen tilaukset.


![V2V kaavio](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien VNet VNet yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Seuraavassa taulukossa on tällä hetkellä käytettävissä käyttöönoton mallit ja viestintämenetelmien VNet VNet määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>Tietoja VNet VNet yhteyksistä

Yhteyden muodostaminen toiseen virtual virtual verkon (VNet VNet) on samanlainen kuin yhdistäminen VNet paikallisen sivuston sijainnissa. Yhteyksien molempien tietotyyppien käyttää Azure VPN-yhdyskäytävän suojatun tunnelin IP/IKE. Voit yhdistää VNets voi olla eri alueilla. Tai eri tilaukset. Voit myös yhdistää VNet VNet tietoliikenteen usean sivuston määrityksiä. Näillä oikeuksilla muodostat rajat verkkotopologioita, joka yhdistää paikallista connectivity väliset VPN-yhteyttä, kuten seuraavassa kaaviossa on esitetty:


![Tietoja yhteyksistä](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Miksi yhteyden virtual verkkoja?

Voit yhdistää virtual verkkojen seuraavista syistä:

- **Toimintojen välinen alueen geo-redundanssin ja geo tavoitettavuus**
    - Voit määrittää oman geo replikoinnin tai synkronoinnin suojattua yhteyttä siirtymättä Internetiin yhteydessä oleva päätepisteet päälle.
    - Azure liikenteen hallinta ja kuormituksen voit määrittää erittäin käytettävissä kuormituksen vikasietoisuusominaisuuksilla geo useita Azure alueiden välillä. Tärkeää esimerkki on määrittäminen SQL aina käyttöön käytettävyys ryhmiin levittäminen useita Azure alueiden välillä.

- **Alueellisten monitasoisten sovellusten eristystaso tai järjestelmänvalvojan reunaa**
    - Samalla alueella voit määrittää monitasoisten sovellusten useita virtual verkkoja yhdistää vuoksi eristystaso tai järjestelmänvalvojan vaatimusten kanssa.


### <a name="vnet-to-vnet-faq"></a>VNet VNet usein kysytyt kysymykset

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Toimintatapaa vaiheista kannattaa käyttää?

Tässä artikkelissa on kaksi eri tiedostojoukot vaiheet. Yksi toimintaohjeiden [, jotka sijaitsevat saman tilauksen VNets](#samesub)ja toinen [eri tilaukset, jotka sijaitsevat VNets](#difsub)varten. Määrittää avaimen ero on, voit luoda ja määrittää kaikki VPN ja yhdyskäytävän resurssit saman PowerShell-istunnossa.

Tässä artikkelissa kuvattuja olla, joka on määritetty jokaisen osan alussa. Jos jo käsitellään aiemmin VNets, muokkaa muuttujaa vastaamaan oman ympäristön asetuksia. 

![Molemmat yhteydet](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Yhteyden VNets, jotka ovat samassa tilaus

![V2V kaavio](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Ennen aloittamista
    
Ennen kuin aloitat, sinun täytyy asentaa Azure Resurssienhallinta PowerShell cmdlet-komentoja. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.

### <a name="Step1"></a>Vaihe 1 - IP-osoitealueet suunnitteleminen


Seuraavissa vaiheissa on luoda kaksi virtual verkkojen sekä niiden vastaaviin yhdyskäytävän aliverkosta ja määrityksiä. Nyt luoda VPN-yhteyden kahden VNets välillä. On tärkeää suunnitella verkon määritysten IP-osoitealueet. Ota huomioon, varmista, ettei mikään VNet alueita tai paikalliseen verkkoon kirjauduttaessa alueiden päällekkäin minkäänlaista.

Käytämme esimerkeissä seuraavat arvot:

**TestVNet1 arvot:**

- VNet nimi: TestVNet1
- Resurssiryhmä: TestRG1
- Sijainti: Itä US
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Taustajärjestelmä: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- DNS-palvelin: 8.8.8.8
- GatewayName: VNet1GW
- Julkiseen IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**TestVNet4 arvot:**

- VNet nimi: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Taustajärjestelmä: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Resurssiryhmä: TestRG4
- Sijainti: Länsi USA
- DNS-palvelin: 8.8.8.8
- GatewayName: VNet4GW
- Julkiseen IP: VNet4GWIP
- VPNType: RouteBased
- Yhteyden: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Vaihe 2 – Luo ja määritä TestVNet1

1. Määritellä oman muuttujat

    Aloita ilmoittamalla muuttujat. Tässä esimerkissä ilmoittaa arvot käytön tämän Harjoitus muuttujat. Useimmissa tapauksissa sinun on korvattava arvot omalla. Voit kuitenkin käyttää muuttujia, jos käytössäsi on tämäntyyppinen kokoonpano tutustutaan vaiheet. Muokkaa muuttujia tarvittaessa ja valitse kopioi ja liitä ne PowerShell console.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Tilauksen yhdistäminen

    Siirry PowerShell-tilassa voit resurssien hallinnan Cmdlet-komentoja. Avaa PowerShell-konsolin ja muodostaa yhteyden tiliisi. Seuraavassa esimerkissä avulla voit tarkastella:

        Login-AzureRmAccount

    Tarkista tilaukset-tilin.

        Get-AzureRmSubscription 

    Määritä tilaus, jota haluat käyttää.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Luo uusi resurssiryhmä

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Luo TestVNet1 aliverkon määritykset

    Tässä esimerkissä luodaan virtual verkon nimeltä TestVNet1 ja kolme aliverkosta, yksi nimeltä GatewaySubnet, yksi kutsuttu edusta ja yksi kutsuttu Taustajärjestelmä. Kun korvaat arvoja, on tärkeää nimi aina yhdyskäytävän aliverkon erityisesti GatewaySubnet. Jos nimeät sen jotain muuta, yhdyskäytävän luominen epäonnistuu. 

    Seuraavassa esimerkissä muuttujat, jotka voit määrittää aiemmin. Tässä esimerkissä yhdyskäytävän aliverkon on käytössä /27. Vaikka ei voi luoda yhdyskäytävän aliverkon /29 mahdollisimman pieni, on suositeltavaa, että luot suurempi aliverkon, joka sisältää useita osoitteita valitsemalla vähintään /28 tai /27. Tämä sallii tarpeeksi osoitteiden sopimaan mahdollista lisämäärityksiä, haluat ehkä myöhemmin. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. Luo TestVNet1

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Pyydä julkiseen IP-osoite

    Pyydä julkinen IP-osoite, että VNet luodaan yhdyskäytävän jaetaan. Huomaa, että AllocationMethod on dynaaminen. Et voi määrittää IP-osoite, jota haluat käyttää. Se on kohdistettu dynaamisesti käyttämäsi yhdyskäytävän. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Luo yhdyskäytävä-asetukset

    Yhdyskäytävän määrittäminen määrittää aliverkon ja käyttämään julkiseen IP-osoite. Esimerkki avulla voit luoda yhdyskäytävän määritykset. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Luo yhdyskäytävä TestVNet1

    Tässä vaiheessa voit luoda virtuaalisen yhdyskäytävien oman TestVNet1. VNet VNet määritykset edellyttävät RouteBased VpnType. Luoda yhdyskäytävän voi viedä aikaa (45 minuuttia tai pidempään).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Vaihe 3: Luo ja määritä TestVNet4

Kun olet määrittänyt TestVNet1, luoda TestVNet4. Noudata alla arvojen korvaaminen omalla tarpeen mukaan. Tässä vaiheessa voidaan toteuttaa saman PowerShell istunnosta, koska se on saman tilauksen.

1. Määritellä oman muuttujat

    Varmista arvojen korvaaminen, jota haluat käyttää kokoonpanosi niistä.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Ennen jatkamista, varmista, että olet muodostanut edelleen tilauksen 1.

2. Luo uusi resurssiryhmä

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Luo TestVNet4 aliverkon määritykset

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. Luo TestVNet4

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Pyydä julkiseen IP-osoite

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Luo yhdyskäytävä-asetukset

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. TestVNet4 yhdyskäytävän luominen

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Vaihe 4 – Yhdistä yhdyskäytävät

1. Hae sekä VPN-yhdyskäytävät

    Tässä esimerkissä koska molemmat yhdyskäytävät ovat saman tilauksen tämä vaihe voidaan suorittaa saman PowerShell-istunnossa.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. TestVNet1 TestVNet4 yhteyden luominen

    Tässä vaiheessa luot yhteyden TestVNet1 TestVNet4. Näet jaetun avaimen viittaus seuraavissa esimerkeissä. Voit käyttää omaa arvot jaetun avaimen. Tärkeintä on, että jaetun avaimen on oltava samat molemmat yhteydet. Yhteyden luominen voi kestää hetki suorittamiseen.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. TestVNet4 TestVNet1 yhteyden luominen

    Tämä vaihe on jokin edellä, mutta olet luomassa yhteyden TestVNet4 TestVNet1. Varmista, että jaetut avaimet.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Yhteys on muodostettu muutaman minuutin kuluttua.

4. Tarkista yhteys. Kohdassa [tarkistaminen yhteys](#verify).


## <a name="difsub"></a>Yhteyden VNets, jotka ovat eri tilaukset


![V2V kaavio](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

Tässä skenaariossa muodostetaan TestVNet1 ja TestVNet5. TestVNet1 ja TestVNet5 sijaitsevat eri tilauksen. Tämän määrityksen vaiheet Lisää muita VNet VNet-yhteys voit muodostaa yhteyden TestVNet5 TestVNet1. 

Erona on, että jotkin määritysvaiheet on suoritettava eri PowerShell-istunnossa toisen tilauksen yhteydessä. Erityisesti kun kaksi tilaukset kuuluvat eri organisaatioissa. 

Yllä olevassa luettelossa edellisen vaiheen edelleen ohjeita. Sinun on suoritettava [vaiheessa 1](#Step1) ja [vaiheessa 2](#Step2) , voit luoda ja määrittää TestVNet1 ja VPN-yhdyskäytävän TestVNet1. Kun olet suorittanut vaiheessa 1 ja vaiheessa 2, jatka vaiheeseen 5 Luo TestVNet5.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Vaihe 5 – Tarkista Lisää IP-osoitealueet

On tärkeää varmistaa, että uusi virtual verkon TestVNet5, IP-osoitetilaa ei ole päällekkäinen VNet alueiden tai paikalliseen verkkoon kirjauduttaessa yhdyskäytävän alueiden avulla. 

Tässä esimerkissä virtual verkot voivat kuulua eri organisaatioissa. Tässä voit käyttää seuraavia arvoja TestVNet5 varten:

**TestVNet5 arvot:**

- VNet nimi: TestVNet5
- Resurssiryhmä: TestRG5
- Sijainti: Japani Itä
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Taustajärjestelmä: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- DNS-palvelin: 8.8.8.8
- GatewayName: VNet5GW
- Julkiseen IP: VNet5GWIP
- VPNType: RouteBased
- Yhteyden: VNet5toVNet1
- ConnectionType: VNet2VNet

**TestVNet1 muita arvoja:**

- Yhteyden: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Vaihe 6 - luominen ja määrittäminen TestVNet5

Tämä vaihe on tehtävä kontekstissa uuteen tilaukseen. Tämän osan voidaan suorittaa eri organisaatiossa, joka omistaa tilauksen järjestelmänvalvoja.

1. Määritellä oman muuttujat

    Varmista arvojen korvaaminen, jota haluat käyttää kokoonpanosi niistä.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Yhteyden muodostaminen tilauksen 5

    Avaa PowerShell-konsolin ja muodostaa yhteyden tiliisi. Seuraavassa esimerkissä avulla voit tarkastella:

        Login-AzureRmAccount

    Tarkista tilaukset-tilin.

        Get-AzureRmSubscription 

    Määritä tilaus, jota haluat käyttää.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Luo uusi resurssiryhmä

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Luo TestVNet4 aliverkon määritykset
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. Luo TestVNet5

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Pyydä julkiseen IP-osoite

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Luo yhdyskäytävä-asetukset

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. TestVNet5 yhdyskäytävän luominen

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Vaihe 7 – yhteyden yhdyskäytävät

Tässä esimerkissä koska yhdyskäytävät ovat eri-tilauksessa on olet Jaa tämä vaihe kaksi PowerShell-istunnot merkitty [tilauksen 1] ja [tilauksen 5].

1. **[Tilauksen 1]** Hae VPN-yhdyskäytävän tilauksen 1

    Varmista, että, kirjaudu sisään ja muodosta yhteys tilauksen 1.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Kopioi tulosteen seuraavat osat ja lähetä ne järjestelmänvalvoja ja tilauksen 5 sähköpostilla tai muulla tavalla.

        $vnet1gw.Name
        $vnet1gw.Id

    Nämä kaksi elementit on arvoja samalla tavalla kuin seuraavassa esimerkissä tulos:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Tilauksen 5]** Hae VPN-yhdyskäytävän tilauksen 5

    Varmista, että, kirjaudu sisään ja muodosta yhteys tilauksen 5.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Kopioi tulosteen seuraavat osat ja lähetä ne järjestelmänvalvoja ja tilauksen 1 sähköpostilla tai muulla tavalla.

        $vnet5gw.Name
        $vnet5gw.Id

    Nämä kaksi elementit on arvoja samalla tavalla kuin seuraavassa esimerkissä tulos:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Tilauksen 1]** TestVNet1 TestVNet5 yhteyden luominen

    Tässä vaiheessa luot yhteyden TestVNet1 TestVNet5. Erona on vain kyseisen $vnet5gw ei saada suoraan, koska se on eri tilauksen. Haluat luoda uuden PowerShell objektin arvoilla tiedoksi tilauksen 1 edellä kuvatulla tavalla. Käytä alla olevassa esimerkissä. Korvaa nimi, tunnus ja jaetun avaimen oman arvoilla. Tärkeintä on, että jaetun avaimen on oltava samat molemmat yhteydet. Yhteyden luominen voi kestää hetki suorittamiseen.

    Varmista, että voit muodostaa yhteyden tilauksen 1. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Tilauksen 5]** TestVNet5 TestVNet1 yhteyden luominen

    Tämä vaihe on jokin edellä, mutta olet luomassa yhteyden TestVNet5 TestVNet1. Tilauksen 1 saatu arvojen perusteella PowerShell-objektin luominen samoja ohjeita koskee tähän paikan päällä. Tässä vaiheessa muista, että jaettu näppäimet vastaavat.

    Varmista, että voit muodostaa yhteyden tilauksen 5.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Yhteyden tarkistaminen


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Seuraavat vaiheet

- Kun yhteys on valmis, voit lisätä näennäiskoneiden virtual verkkoihin. Lisätietoja on kohdassa [Create Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ohjeita.
- Saat lisätietoja erityisen [Erityisen yleiskatsaus](vpn-gateway-bgp-overview.md) ja [erityisen määrittämisestä](vpn-gateway-bgp-resource-manager-ps.md). 

