Tähän käytetään VNet, alla arvojen perusteella. Lisäasetuksia ja nimet ovat myös Jäsennettyjen tässä luettelossa. Olemme Älä käytä tämän luettelon suoraan ohjeita, vaikka lisäämme muuttujat tämän luettelon arvojen perusteella. Voit kopioida luettelon käyttää viitteenä-arvojen korvaaminen omalla.

Määritysten viittaus-luettelossa jokin seuraavista:
    
- Virtuaalinen verkkonimi = "TestVNet"
- Virtuaalinen verkko-osoitetilaa = 192.168.0.0/16
- Resurssiryhmä = "TestRG"
- Subnet1 nimi = "FrontEnd" 
- Subnet1 osoitetilaa = "192.168.0.0/16"
- Yhdyskäytävän aliverkon nimi: "GatewaySubnet" yhdyskäytävän aliverkon *GatewaySubnet*aina nimeä.
- Yhdyskäytävän aliverkon osoitetilaa = "192.168.200.0/26"
- Alueen = "Itä US"
- Yhdyskäytävän nimi = "GW"
- Yhdyskäytävän IP nimi = "GWIP"
- Yhdyskäytävän IP-määritys nimi = "gwipconf"
-  Tyyppi = "ExpressRoute" tällaista tarvitaan ExpressRoute-määritys.
- Yhdyskäytävän julkiseen IP-nimi = "gwpip"


## <a name="add-a-gateway"></a>Lisää yhdyskäytävän

1. Yhdistä Azure-tilaukseen. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Määritellä tämän Harjoitus oman muuttujat. Tässä esimerkissä käyttää käytön muuttujat alla olevassa esimerkissä. Varmista Muokkaa tätä mukaiseksi, jota haluat käyttää asetuksia. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Tallentaa VPN-objektin muuttujana.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Lisää yhdyskäytävän aliverkon näennäinen verkkoon. Yhdyskäytävän aliverkon nimen on oltava "GatewaySubnet". Haluat luoda yhdyskäytävän, joka on /27 tai suurempi (/ 26/25, jne.).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. Määritä asetukset.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Tallentaa yhdyskäytävän aliverkon muuttujana.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Pyydä julkiseen IP-osoite. Ennen kuin luot yhdyskäytävä on pyydetty IP-osoite. Et voi määrittää IP-osoite, jota haluat käyttää; dynaamisesti jaetaan. Käytät tätä IP-osoitetta seuraavan osion määritys. AllocationMethod on oltava dynaaminen.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Voit luoda oman Gatewayn määritysten. Yhdyskäytävän määrittäminen määrittää aliverkon ja käyttämään julkiseen IP-osoite. Tässä vaiheessa määrität, jota käytetään, kun luot yhdyskäytävän määritykset. Tämä vaihe ei todellisuudessa Luo yhdyskäytävä-objekti. Esimerkki alla avulla voit luoda yhdyskäytävän määritykset. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Luo yhdyskäytävä. Tässä vaiheessa **-GatewayType** on erityisen tärkeää. Voit käyttää arvoa **ExpressRoute**. Huomaa, että näiden cmdlet-komennot suorittamisen jälkeen, yhdyskäytävän voi kestää 20 minuutin ajan tai Lisää luomiseen.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Tarkista yhdyskäytävän on luotu

Alla olevassa-komennon avulla voit varmistaa, että yhdyskäytävän luodaan.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Yhdyskäytävän koon muuttaminen

On useita [Yhdyskäytävän tuotteissa](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Seuraavalla komennolla voit muuttaa yhdyskäytävän SKU milloin tahansa.

>[AZURE.IMPORTANT] Tämä komento ei toimi UltraPerformance Gatewayn. Voit muuttaa käyttämäsi UltraPerformance yhdyskäytävän yhdyskäytävän, poista ensin aiemmin ExpressRoute yhdyskäytävän ja luo sitten uusi UltraPerformance-yhdyskäytävä. Muuntaminen oman Gatewayn UltraPerformance yhdyskäytävän, poista ensin UltraPerformance yhdyskäytävän ja luo uusi yhdyskäytävä.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Poista yhdyskäytävä

Poista yhdyskäytävä alla-komennon avulla

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
