### <a name="to-verify-your-connection-by-using-powershell"></a>Voit tarkistaa yhteytesi PowerShell-toiminnolla

Voit varmistaa, että yhteys onnistui avulla `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet-kanssa tai ilman `-Debug`. 

1. Käytä seuraava cmdlet-Esimerkki määrittäminen vastaamaan omia arvot. Valitse pyydettäessä "A" suorittamiseksi all (kaikki). Esimerkissä `-Name` viittaa nimeen, jotka olet luonut ja testattava yhteyttä.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Kun cmdlet-komento on päättynyt, tarkastele arvoja. Seuraavassa esimerkissä yhteyden tila näkyy 'Yhteys' ja näet tunkeutumisen ja juniin tavun.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Voit tarkistaa yhteytesi mukaan Azure-portaalissa

Voit tarkastella yhteyden tilan Azure-portaalissa siirtymällä yhteys. On useita tapoja toiminto. Seuraavissa vaiheissa kuvataan yksi tapa Siirry yhteys ja tarkista.

1. [Azure portal](http://portal.azure.com)Valitse **kaikki resurssit** ja siirry VPN-yhdyskäytävä.
2. Valitse VPN-Gatewayn sivu **yhteydet**. Näet kunkin yhteyden tilan.
3. Napsauta nimeä, jonka haluat avata **Essentials**vahvistaa yhteyden. Voit tarkastella lisätietoja Essentials-yhteyden tietoja. **Tila** on 'Onnistui' ja "Yhdistetty", kun olet tehnyt onnistui.

    ![Tarkista yhteys](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)