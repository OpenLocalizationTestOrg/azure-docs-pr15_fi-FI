## <a name="scenario"></a>Skenaario

Esitä paremmin luomisesta VNet ja aliverkosta, tämä asiakirja käyttämällä alla skenaario.

![VNet-skenaario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Tässä skenaariossa luodaan VNet, nimeltä **TestVNet** varattu CIDR tekstialueen **192.168.0.0./16**kanssa. Oman VNet sisältää seuraavat aliverkosta: 

- **Edusta**-käyttämällä **192.168.1.0/24** sen CIDR palkkina.
- **Taustatietokannan**, käyttämällä **192.168.2.0/24** sen CIDR palkkina.

 