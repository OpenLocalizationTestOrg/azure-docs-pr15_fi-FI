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