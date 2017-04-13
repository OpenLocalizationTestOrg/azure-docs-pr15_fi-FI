### <a name="noconnection"></a>Voit lisätä tai poistaa etuliitteiden - yhteyttä ei Gatewayn

- **Voit lisätä** muita osoitteiden etuliitteiden paikalliseen verkkoon, jotka olet luonut, mutta, joka ei Gatewayn vielä on yhdyskäytävän yhteys, käytä alla olevassa esimerkissä. Muista muuttaa omia arvoja.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Voit poistaa** osoitteen etuliite kuin paikalliseen verkkoon kirjauduttaessa yhdyskäytävään, joka ei ole VPN-yhteyden, käytä alla olevassa esimerkissä. Jättämällä pois etuliitteen, jota et enää tarvitse. Tässä esimerkissä on enää tarvitse etuliite 20.0.0.0/24 (kuin edellisessä esimerkissä), niin päivitämme paikallisen verkon yhdyskäytävän ja jätä pois, että etuliite.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Voit lisätä tai poistaa etuliitteiden - aiemmin yhdyskäytävän yhteys

Jos olet luonut yhdyskäytävän yhteys ja haluat lisätä tai poistaa IP-osoitteiden etuliitteiden sisältyvät paikalliseen verkkoon kirjauduttaessa yhdyskäytävän, tarvitset seuraavat toimet järjestyksessä. Tämä aiheuttaa joidenkin käyttökatkot VPN-yhteyden. Oman etuliitteiden päivityksessä ensin poistettava yhteys, muokata etuliitteitä ja luo sitten uusi yhteys. Alla olevassa esimerkissä muista muuttaa omia arvoja.

>[AZURE.IMPORTANT] Älä poista VPN-yhdyskäytävä. Jos teet näin, sinun on palaa läpi vaiheet ja luoda sekä määrittää paikallisen reitittimen uusia asetuksia.
 
1. Poista yhteys.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Muokkaa osoitteiden etuliitteiden paikalliseen verkkoon kirjauduttaessa Gatewayn.

    Voit määrittää muuttuja LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Muokkaa etuliitteitä.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Yhteyden muodostaminen. Tässä esimerkissä on määritetään IP-yhteyden tyyppi. Kun yhteys uudelleen, käytä tietoyhteyden kohdalla, joka on määritetty kokoonpanon. Saat lisätietoja yhteystyypit [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) -sivu.

    Voit määrittää muuttuja VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Yhteyden muodostaminen. Huomaa, että tässä esimerkissä käytetään muuttuja $local, jolla voit määrittää edellisessä vaiheessa.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
