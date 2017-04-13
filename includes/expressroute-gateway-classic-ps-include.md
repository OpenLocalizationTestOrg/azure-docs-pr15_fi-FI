Sinun on luotava VNet ja yhdyskäytävän aliverkon ensin ennen seuraavien tehtävien parissa. On lisätietoja artikkelissa [perinteinen portaalissa Virtual verkon määrittäminen](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) .   

## <a name="add-a-gateway"></a>Lisää yhdyskäytävän

Alla olevassa-komennon avulla voit luoda yhdyskäytävän. Muista korvaa arvot itse.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Tarkista yhdyskäytävän on luotu

Alla olevassa-komennon avulla voit varmistaa, että yhdyskäytävän luodaan. Tämä komento hakee myös yhdyskäytävän ID-tunnus, jota tarvitset muihin toimintoihin.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Yhdyskäytävän koon muuttaminen

On useita [Yhdyskäytävän tuotteissa](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Seuraavalla komennolla voit muuttaa yhdyskäytävän SKU milloin tahansa.

>[AZURE.IMPORTANT] Tämä komento ei toimi UltraPerformance Gatewayn. Voit muuttaa käyttämäsi UltraPerformance yhdyskäytävän yhdyskäytävän, poista ensin aiemmin ExpressRoute yhdyskäytävän ja luo sitten uusi UltraPerformance-yhdyskäytävä. Muuntaminen oman Gatewayn UltraPerformance yhdyskäytävän, poista ensin UltraPerformance yhdyskäytävän ja luo uusi yhdyskäytävä. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Poista yhdyskäytävä

Poista yhdyskäytävä alla-komennon avulla

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>