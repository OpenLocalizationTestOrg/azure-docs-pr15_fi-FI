
Määrittäminen VPN-laitteessa, sinun on julkinen IP-osoite VPN-yhdyskäytävän määrittämiseen paikallisen VPN-laite. Laitteen määrittäminen ja käsittely tietyn kokoonpanotietoja laitteen valmistajalta. Katso lisätietoja VPN-laitteet, jotka toimivat hyvin Azure [VPN-laitteet](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) .

Voit hakea käyttämäsi VPN-yhdyskäytävän PowerShellin julkiseen IP-osoitteen avulla seuraavassa esimerkissä:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Voit tarkastella julkinen IP-osoite VPN-Gatewayn myös Azure-portaalissa. Siirry **Virtual verkon yhdyskäytävät**ja valitse sitten oman yhdyskäytävän nimeä.