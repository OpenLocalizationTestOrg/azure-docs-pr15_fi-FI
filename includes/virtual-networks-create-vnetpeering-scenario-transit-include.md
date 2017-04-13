## <a name="service-chaining---transit-through-peered-vnet"></a>Palvelun Ketjutusta - salataanko siirrettävät peered VNet kautta

Vaikka järjestelmän tiet käyttäminen helpottaa tietoliikenteen automaattisesti käyttöönoton, on tapauksia, joissa haluat hallita pakettien reititys näennäinen laitteen kautta.
Tässä skenaariossa on kaksi VNets Tilauksen, HubVNet ja VNet1 kuvatulla tavalla seuraavassa kuvassa. Voit ottaa käyttöön verkon Virtual Appliance(NVA)-VNet HubVNet. Jälkeen VNet peering HubVNet ja VNet1 välillä, käyttäjän määrittämät tiet määrittäminen ja seuraavan pisteen NVA voit määrittää HubVNet.

![NVA salataanko siirrettävät](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Yksinkertaisuuden oletetaan, että kaikki VNets tähän on saman tilauksen. Mutta se toimii myös rajat-tilaus-tilanne.

Voit ottaa käyttöön salataanko siirrettävät reititys avaimen-ominaisuus on "Salli välittää liikenne"-parametrin. Näin hyväksymällä ja lähettäminen tietoliikenne / NVA peered VNet.  
