Jos haluat muokata yhdyskäytävän IP-osoite, käytä `New-AzureRmVirtualNetworkGatewayConnection` cmdlet-komento. Kun pidät paikalliseen verkkoon kirjauduttaessa yhdyskäytävän nimeä täsmälleen sama kuin aiemmin, korvaa asetukset. Tällä hetkellä toiminnon Aseta-cmdlet-komento ei tue muokkaaminen yhdyskäytävän IP-osoite.

### <a name="gwipnoconnection"></a>Yhdyskäytävän IP-osoite - yhteyttä ei Gatewayn muokkaamisesta

Voit päivittää oman paikalliseen verkkoon kirjauduttaessa yhdyskäytävään, joka ei ole vielä yhteydessä yhdyskäytävän IP-osoite, käytä alla olevassa esimerkissä. Voit myös päivittää osoitteiden etuliitteiden samanaikaisesti. Voit määrittää asetukset korvaa olemassa olevat asetukset. Muista käyttää aiemmin paikalliseen verkkoon kirjauduttaessa yhdyskäytävän nimi. Jos luot uuden paikallisen yhdyskäytävien, korvaaminen ei ole aiemmin luotuun.

Käytä seuraavassa esimerkissä korvaa arvot itse.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Yhdyskäytävän IP-osoite - aiemmin yhdyskäytävän yhteys muokkaamisesta

Jos on jo yhdyskäytävän yhteys, ne on ensin yhteys. Tämän jälkeen voit muokata yhdyskäytävän IP-osoite ja luo uusi yhteys. Tämä aiheuttaa joidenkin käyttökatkot VPN-yhteyden.


>[AZURE.IMPORTANT] Älä poista VPN-yhdyskäytävä. Jos Tee, jotta voit siirtyä takaisin läpi vaiheet ja luoda sekä uudelleen paikallisen reitittimen IP-osoite, joka määritetään juuri luomasi yhdyskäytävä on.
 

1. Poista yhteys. Voit etsiä haluamasi yhteys käyttämällä `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet-komento.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Muokkaa GatewayIpAddress-arvo. Voit myös muokata osoitteiden etuliitteiden tällä hetkellä tarvittaessa. Huomaa, että tämä korvaa aiemmin paikalliseen verkkoon kirjauduttaessa yhdyskäytävän asetusten. Käyttää aiemmin paikalliseen verkkoon kirjauduttaessa yhdyskäytävän nimi niin, että asetukset korvaavat muokattaessa. Jos luot uuden paikalliseen verkkoon kirjauduttaessa yhdyskäytävän muokkaaminen ei ole aiemmin luotuun.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Yhteyden muodostaminen. Tässä esimerkissä on määritetään IP-yhteyden tyyppi. Kun yhteys uudelleen, käytä tietoyhteyden kohdalla, joka on määritetty kokoonpanon. Saat lisätietoja yhteystyypit [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) -sivu.  Voit hankkia VirtualNetworkGateway nimi, voit avata `Get-AzureRmVirtualNetworkGateway` cmdlet-komento.

    Määritä muuttujat:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Yhteyden luominen:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

